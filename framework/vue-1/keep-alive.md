# keep-alive

## keep-alive介绍

keep-alive是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在父组件链中；使用keep-alive包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。

## 用法

* 在动态组件中的应用

```markup
<keep-alive :include="whiteList" :exclude="blackList" :max="amount">
  <component :is="currentComponent"></component>
</keep-alive>
```

* 在vue-router中的应用

```markup
<keep-alive :include="whiteList" :exclude="blackList" :max="amount">
  <router-view></router-view>
</keep-alive>
```

`include`定义缓存白名单，keep-alive会缓存命中的组件；`exclude`定义缓存黑名单，被命中的组件将不会被缓存；`max`定义缓存组件上限，超出上限使用[LRU的策略](https://baike.baidu.com/item/LRU)置换缓存数据。\


## 实现

```javascript
// src/core/components/keep-alive.js
export default {
  name: 'keep-alive',
  abstract: true, // 判断当前组件虚拟dom是否渲染成真是dom的关键

  props: {
    include: patternTypes, // 缓存白名单
    exclude: patternTypes, // 缓存黑名单
    max: [String, Number] // 缓存的组件实例数量上限
  },

  created () {
    this.cache = Object.create(null) // 缓存虚拟dom
    this.keys = [] // 缓存的虚拟dom的健集合
  },

  destroyed () {
    for (const key in this.cache) { // 删除所有的缓存
      pruneCacheEntry(this.cache, key, this.keys)
    }
  },

  mounted () {
    // 实时监听黑白名单的变动
    this.$watch('include', val => {
      pruneCache(this, name => matches(val, name))
    })
    this.$watch('exclude', val => {
      pruneCache(this, name => !matches(val, name))
    })
  },

  render () {
    // 先省略...
  }
}

```

如上所示定义了一个`abstract: true` 的组件

keep-alive在它生命周期内定义了三个钩子函数：

*   **created**

    初始化两个对象分别缓存VNode（虚拟DOM）和VNode对应的键集合
*   **destroyed**

    删除`this.cache`中缓存的VNode实例。我们留意到，这里不是简单地将`this.cache`置为`null`，而是遍历调用`pruneCacheEntry`函数删除。`pruneCacheEntry`会调用内部的$destroy钩子函数
*   **mounted**

    在`mounted`这个钩子中对`include`和`exclude`参数进行监听，然后实时地更新（删除）`this.cache`对象数据。`pruneCache`函数的核心也是去调用`pruneCacheEntry`。

```javascript
 // src/core/components/keep-alive.js
  render () {
    const slot = this.$slots.default
    const vnode: VNode = getFirstComponentChild(slot) // 找到第一个子组件对象
    const componentOptions: ?VNodeComponentOptions = vnode && vnode.componentOptions
    if (componentOptions) { // 存在组件参数
      // check pattern
      const name: ?string = getComponentName(componentOptions) // 组件名
      const { include, exclude } = this
      if ( // 条件匹配
        // not included
        (include && (!name || !matches(include, name))) ||
        // excluded
        (exclude && name && matches(exclude, name))
      ) {
        return vnode
      }

      const { cache, keys } = this
      const key: ?string = vnode.key == null // 定义组件的缓存key
        // same constructor may get registered as different local components
        // so cid alone is not enough (#3269)
        ? componentOptions.Ctor.cid + (componentOptions.tag ? `::${componentOptions.tag}` : '')
        : vnode.key
      if (cache[key]) { // 已经缓存过该组件
        vnode.componentInstance = cache[key].componentInstance
        // make current key freshest
        remove(keys, key)
        keys.push(key) // 调整key排序
      } else {
        cache[key] = vnode // 缓存组件对象
        keys.push(key)
        // prune oldest entry
        if (this.max && keys.length > parseInt(this.max)) { // 超过缓存数限制，将第一个删除
          pruneCacheEntry(cache, keys[0], keys, this._vnode)
        }
      }

      vnode.data.keepAlive = true // 渲染和执行被包裹组件的钩子函数需要用到
    }
    return vnode || (slot && slot[0])
  }
```

第一步：获取keep-alive包裹着的第一个子组件对象及其组件名；

第二步：根据设定的黑白名单（如果有）进行条件匹配，决定是否缓存。不匹配，直接返回组件实例（VNode），否则执行第三步；

第三步：根据组件ID和tag生成缓存Key，并在缓存对象中查找是否已缓存过该组件实例。如果存在，直接取出缓存值并更新该`key`在`this.keys`中的位置（更新key的位置是实现LRU置换策略的关键），否则执行第四步；

第四步：在`this.cache`对象中存储该组件实例并保存`key`值，之后检查缓存的实例数量是否超过`max`的设置值，超过则根据LRU置换策略删除最近最久未使用的实例（即是下标为0的那个key）。

第五步：最后并且很重要，将该组件实例的`keepAlive`属性值设置为`true`。

## 区别与普通组件的控制

### 不会生成真正的dom节点

keep-alive不会生成真正的dom节点。这是Vue在初始化声明周期时，为组件实例建立父子关系会根据`abstract`属性决定是否忽略某个组件。

### 只执行一次的钩子

```javascript
// src/core/vdom/create-component.js
const componentVNodeHooks = {
  init (vnode: VNodeWithData, hydrating: boolean): ?boolean {
    if (
      vnode.componentInstance &&
      !vnode.componentInstance._isDestroyed &&
      vnode.data.keepAlive
    ) {
      // kept-alive components, treat as a patch
      const mountedNode: any = vnode // work around flow
      componentVNodeHooks.prepatch(mountedNode, mountedNode)
    } else {
      const child = vnode.componentInstance = createComponentInstanceForVnode(
        vnode,
        activeInstance
      )
      child.$mount(hydrating ? vnode.elm : undefined, hydrating)
    }
  }
  // ...
}
```

当`vnode.componentInstance`和`keepAlive`同时为truly值时，不再进入`$mount`过程，那`mounted`之前的所有钩子函数（`beforeCreate`、`created`、`mounted`）都不再执行。

#### 可重复的activated

在`patch`的阶段，最后会执行`invokeInsertHook`函数，而这个函数就是去调用组件实例（VNode）自身的`insert`钩子
