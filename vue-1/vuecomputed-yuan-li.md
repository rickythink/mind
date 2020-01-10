# vue-computed原理

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

