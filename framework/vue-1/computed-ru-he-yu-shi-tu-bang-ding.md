# computed如何与视图绑定

## computed 初始化

1. 合并配置
2. initState
3. initComputed lazy: true
4. 遍历computed上所有的属性，并new watcher

```javascript
for (const key in computed) {
  if (!isSSR) {
    watchers[key] = new Watcher(
      vm,
      getter || noop,
      noop,
      computedWatcherOptions
    )
  }
  // ...
  if (!(key in vm)) {
    defineComputed(vm, key, userDef)
  } else if (process.env.NODE_ENV !== 'production') {
    // ...
  }
}

// watcher.js
class Watcher {
  // ...
  constructor (
    vm: Component,
    expOrFn: string | Function,
    cb: Function,
    options?: ?Object,
    isRenderWatcher?: boolean
  ) {
    // ...
    if (options) {
    	// ...
    	this.lazy = !!options.lazy
      // ...
    }
    this.dirty = this.lazy // for lazy watchers
      
    if (typeof expOrFn === 'function') {
      this.getter = expOrFn
    } 
		...
    this.value = this.lazy
      ? undefined
      : this.get()
  }
}
```

5\. new Watcher 首次进来，lazy = dirty = true, this.value = undefined

6\. 因为key不在vm上，会defineComputed, 把computed的所有属性都响应式

```javascript
const sharedPropertyDefinition = {
      enumerable: true,
      configurable: true,
      get: noop,
      set: noop
    }
    export function defineComputed (
      target: any,
      key: string,
      userDef: Object | Function
    ) {
      const shouldCache = !isServerRendering() // true
      if (typeof userDef === 'function') {
        sharedPropertyDefinition.get = shouldCache
          ? createComputedGetter(key)
          : createGetterInvoker(userDef)
        sharedPropertyDefinition.set = noop
      } 
      // ...
      Object.defineProperty(target, key, sharedPropertyDefinition)
    }
    
    function createComputedGetter (key) {
      return function computedGetter () {
        const watcher = this._computedWatchers && this._computedWatchers[key]
        if (watcher) {
           // 第一次视图渲染时进入
          if (watcher.dirty) { 
            // 第一次默认为 true
            // 第一次会触发依赖收集, watcher.deps 是当前的所有依赖
            watcher.evaluate() 
          }
          // 第一次上面执行完后，当前Dep.target是当前Render！
          if (Dep.target) { 
    				// 把当前computed watcher的全部作为依赖都逐一添加到render watcher
    				// 因此逐一依赖的subs 也会有render watcher
    				// 那么所有依赖的subs 不仅有了computed watcher
    				// 也有了rener watcher
    				// 没有触发视图渲染的computed不会触发get，也不会到这里
            // 甚至虽然computed-watch触发，但是最终返回值不变
            // 没有导致render刷新，也不会到这里
            watcher.depend() 
          }
          return watcher.value
        }
      }
    }
    
    // watch.js
    evaluate () {
      // 脏数据时才更新
      if (this.dirty) {
        this.value = this.get()
        this.dirty = false
      }
      // 不然读取缓存值
      return this.value
    }

    depend () {
      let i = this.deps.length
      while (i--) {
        this.deps[i].depend()
      }
    }
```

7\. 待到视图渲染时，会读取这个computed属性，由于设置了响应式，会触发上面的 `watcher.evaluate`&#x20;

8\. evaluate 中又调用了`this.get`&#x20;

{% code title="watcher.js" %}
```javascript
get () {
  pushTarget(this)  // 设置 Dep.target 为本watcher
  let value
  const vm = this.vm
  try {
    value = this.getter.call(vm, vm) // 触发已定义data或者props的getter依赖收集
  } catch (e) {
    if (this.user) {
      handleError(e, vm, `getter for watcher "${this.expression}"`)
    } else {
      throw e
    }
  } finally {
    // "touch" every property so they are all tracked as
    // dependencies for deep watching
    if (this.deep) { // 有deep属性就深度遍历
      traverse(value)
    }
    popTarget() // 依赖收集完毕，出栈
    this.cleanupDeps() // 转移watcher最新newDeps到deps上
  }
  return value
}
```
{% endcode %}

这个get，会执行 this.getter.call(vm,vm)。

由于当前的props, datas 已经都响应式了，这样会触发他们的get进行依赖收集

```javascript
Object.defineProperty(obj, key, {
  // ...
  get: function reactiveGetter () {
    const value = getter ? getter.call(obj) : val
    if (Dep.target) {
      dep.depend()
      // ...
    }
    return value
  },
  // ...
}

// dep.js
depend () {
  if (Dep.target) {
    Dep.target.addDep(this) // 目标把自己添加为依赖项
  }
}
// watcher.js
// newDeps: Array<Dep>;
// depIds: SimpleSet;
// newDepIds: SimpleSet;
addDep (dep: Dep) {
  const id = dep.id
  if (!this.newDepIds.has(id)) {
    this.newDepIds.add(id)
    this.newDeps.push(dep)
    if (!this.depIds.has(id)) { // 第一次并没有
      dep.addSub(this) // 传进来的依赖，添加本watcher订阅
    }
  }
}
// dep.js 
addSub (sub: Watcher) { 
  this.subs.push(sub) // 设置订阅的watcher
}

```

{% hint style="info" %}
所谓的dep.depend

就是给当前的 Dep.target 对应的 Watcher 的 newDeps数组追加实例，并且在自身的 dep.sub 数组中添加这个 watcher，以便未来更新的时候进行 dep.notify 通知
{% endhint %}

如此就把所有的computed的属性依赖收集完毕。最后返回dirty=false 和新值。下次直接读取缓存。

9\. 紧着着。因为现在是render-watcher。所有Dep.target变回render，接下来的watcher.depend。会把computed所有依赖都逐一dep.depend到render-watcher中。也即，所有的computed依赖变化后，也会通知render-watcher

## 依赖变化

依赖变化后，如果computed-watcher的subs为0，那么仅需把dirty再置为true即可。因为没有render-watcher使用它，自然不需要刷新视图。

如果有render-watcher监听。那么在nextTick后，对所有的wacher queue进行排序。因为render-watcher id一定大于computed, 会在computed-watcher反复进行watch.run后（也仅设置dirty为true）最后的render-watch执行evaluate, 完成this.get()的读取。

## 参考

> [https://juejin.im/post/5d538d27f265da03e3697803#heading-6](https://juejin.im/post/5d538d27f265da03e3697803#heading-6)
>
> [https://ustbhuangyi.github.io/vue-analysis/v2/reactive/computed-watcher.html#computed](https://ustbhuangyi.github.io/vue-analysis/v2/reactive/computed-watcher.html#computed)



