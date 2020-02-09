# vue-computed原理

> [https://juejin.im/post/5afbfce56fb9a07ac0226f21](https://juejin.im/post/5afbfce56fb9a07ac0226f21)

## 为什么computed可以缓存

vue2.0中，computed是基于watch的。

{% code title="watch实现中函数" %}
```javascript
evaluate () {
    this.value = this.get()
    this.dirty = false
}
```
{% endcode %}

{% code title="computed 实现中的函数" %}
```javascript
//计算属性的getter 获取计算属性的值时会调用
	createComputedGetter (key) {
	  return function computedGetter () {
	  	//获取到相应的watcher
	    const watcher = this._computedWatchers && this._computedWatchers[key]
	    if (watcher) {
	    	//watcher.dirty 参数决定了计算属性值是否需要重新计算，默认值为true，即第一次时会调用一次
	      	if (watcher.dirty) {
	      		/*每次执行之后watcher.dirty会设置为false，只要依赖的data值改变时才会触发
	      		watcher.dirty为true,从而获取值时从新计算*/
	        	watcher.evaluate()
	      	}
	      	//获取依赖
	      	if (Dep.target) {
	        	watcher.depend()
	      	}
	      	//返回计算属性的值
	      	return watcher.value
	    }
	  }
	}
```
{% endcode %}

{% code title="watch 中的update 函数" %}
```javascript
//更新
	update () {
		if (this.lazy) {
  		this.dirty = true
	}else{
		this.run()
	}
```
{% endcode %}

只有watch收集到更新了才会触发computed计算，否则直接返回缓存值

## 总结

computed 本质是一个惰性求值的观察者。

computed 内部实现了一个惰性的 watcher,也就是 computed watcher,computed watcher 不会立刻求值,同时持有一个 dep 实例。

其内部通过 this.dirty 属性标记计算属性是否需要重新求值。

当 computed 的依赖状态发生改变时,就会通知这个惰性的 watcher,

computed watcher 通过 this.dep.subs.length 判断有没有订阅者,

有的话,会重新计算,然后对比新旧值,如果变化了,会重新渲染。 \(**Vue 想确保不仅仅是计算属性依赖的值发生变化，而是当计算属性最终计算的值发生变化时才会触发渲染 watcher 重新渲染，本质上是一种优化。**\)

没有的话,仅仅把 this.dirty = true。 \(**当计算属性依赖于其他数据时，属性并不会立即重新计算，只有之后其他地方需要读取属性的时候，它才会真正计算，即具备 lazy（懒计算）特性。**\)  


