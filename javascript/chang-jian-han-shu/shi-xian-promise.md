# 实现Promise

> 参阅这个，层层递进，写得非常好：[https://zhuanlan.zhihu.com/p/58428287](https://zhuanlan.zhihu.com/p/58428287)

## 极简内核

1. Promise可以new, 所以它是一个类
2. new Promise 传入一个函数，所以constructor是fn
3. fn中会有resolve，我们需要自己定义\_resolve传进去作为方法，因此有fn(this.\_resolve.bind(this))
4. new Promise的实例还可以.then，因此then是Promise的方法
5. 内部维护一个callback，作为传入fn的管理，在调用resolve的时候全部遍历执行

```javascript
class Promise {
	callbacks = [];
	constructor(fn) {
		// 内部的_resolve绑定为外部调用
		fn(this._resolve.bind(this))
	}
	then(onFulfilled){
		this.callbacks.push(onFulfilled)
	}
	_resolve(value){
		this.callbacks.forEach(fn => fn(value))
	}
}
```

## 链式调用

1. 由于promise可以组成then的链式调用，then方法必然也返回promise的实例
2. promise.then是微任务，并不是立即执行，需要增加state作为状态进行执行或者收集的判断
3. promise.then可以传递值给下一个then，因此需要内部维护结果，增加value值
4. promise.then 的 onFulfilled 可以为空，直接resolve之前的值，否知先收集，再统一执行

```javascript
class Promise {
	callbacks = []
	state = 'pending'
	value = undefined

	constructor(fn) {
		fn(this._resolve.bind(this)
	}
	then(onFulfilled = null){
		// 下面的resolve就是新的实例的this._resolve
		return new Promise(resolve => {
			this._handle({ onFulfilled, resolve })
		})
	}
	_handle(callback){
		if (this.state === 'pending') {
			this.callbacks.push(callback)
			return 
		}
		if (!callback.onFulfilled){
			// 未设置onFulfilled, resolve 结果给下一个then
			callback.resolve(this.value)
			return
		}
		const ret = callback.onFulfilled(this.value)
		callback.resolve(ret)
	}
	_resolve(value) {
		// 先切换状态，这样handle里面的判断不会影响
		this.state = 'fulfilled' 
		this.value = value
		// 一个实例的所有callbacks执行完
		this.callbacks.foreach(callback => this._handle(callback))
	}
}
```

## 错误处理

1. 增加reject
2. onRejected也可以为空
3. 即使是有rejected，那也只是设置状态，之前的callback依然要遍历执行完

```javascript
class Promise{
	state = 'pending'
	callbacks = []
	value = undefined
	constructor(fn){
		fn(this._resolve.bind(this), this._reject.bind(this)
	}
	_resolve(value){
		this.state = 'fullfiled'
		this.value = value
		this.callbacks.foreach(callback => this._handle(callback))
	}
	_reject(error){
		this.state = 'rejected'
		this.value = error
		this.callbacks.foreach(callback => this._handle(callback))
	}
	_handle(callback){
		if( this.state === 'pending') {
				this.callbacks.push(callback)
				return
		}
		
		let cb = this.state === 'fullfilled' ? callback.onFulfilled : callback.onRejected
		if (!cb) {
			// 没定义 onFulfilled 或者 onRejected
			cb = this.state === 'fullfilled' ? callback.resolve : callback.reject
			cb(this.value)
			return
		}
		const ret = cb()
		cb(ret)
	}
	then(onFulfilled = null, onRejected = null){
		return new Promise((resolve, reject) => {
				this._handle(onFulfilled, onRejected, resolve, reject)	
			}
	}
}
```

## catch

1. catch也是接收一个函数
2. catch会挂载一个onError, 然后注册到then作为下一个微任务执行
3. **为什么class是一个callbacks数组。这里就是callback数组会有两个callback了。一个是then注册的，一个是catch注册的。其他的then都是注册一个新的实例**

```javascript
class Promise{
	state = 'pending'
	callbacks = []
	value = undefined
	constructor(fn){
		fn(this._resolve.bind(this), this._reject.bind(this)
	}
	_resolve(value){
		this.state = 'fullfiled'
		this.value = value
		this.callbacks.foreach(callback => this._handle(callback))
	}
	_reject(error){
		this.state = 'rejected'
		this.value = error
		this.callbacks.foreach(callback => this._handle(callback))
	}
	_handle(callback){
		if( this.state === 'pending') {
				this.callbacks.push(callback)
				return
		}
		
		let cb = this.state === 'fullfilled' ? callback.onFulfilled : callback.onRejected
		if (!cb) {
			// 没定义 onFulfilled 或者 onRejected
			cb = this.state === 'fullfilled' ? callback.resolve : callback.reject
			cb(this.value)
			return
		}
		const ret = cb()
		cb(ret)
	}
	then(onFulfilled = null, onRejected = null){
		// 每次 then 都会创建新的 Promise 实例
		return new Promise((resolve, reject) => {
				this._handle(onFulfilled, onRejected, resolve, reject)	
			}
	}
	catch(onError){
		// onFulfill为null，仅在onReject里面被调用
		return this.then(null, onError)
	}
}
```

## 实现静态方法

1. Promise.resolve对于一个promise实例，会直接返回该实例，因为它自身就有then方法
2. Promise.resolve对于一个对象属性then为函数的，会创建新Promise实例，把这个函数then 传入resolve
3. Promise.resolve对于一个值(即使是函数），会创建新Promise实例，并resolve这个值

{% hint style="info" %}
thenable函数为：A = { then : function(onFulfilled, onRejected){...}}
{% endhint %}

```javascript
class Promise {
	static resolve(value) {
		if(value && value instanceof Promise){
			return value
		} else if( value && typeof value === 'object' && typeof value.then === 'function') {
			const then = value.then
			return new Promise( resolve => then(resolve))
		} else {
			return new Promise(resolve => resolve(value))
		} 
	}
}
```

## 实现finally

1. finally 不管是onFulfilled 还是 onRejected 都会被调用，因为内部注册到then 上要挂载 两个函数

```javascript
class Promise {
	static resolve(value) {
		if(value && value instanceof Promise){
			return value
		} else if( value && typeof value === 'object' && typeof value.then === 'function') {
			const then = value.then
			return new Promise( resolve => then(resolve))
		}  else {
			return new Promise(resolve => resolve())
		}
	}
	static reject(value) {
    if (value && typeof value === 'object' && typeof value.then === 'function') {
      let then = value.then;
      return new Promise((resolve, reject) => {
        then(reject);
      });

    } else {
      return new Promise((resolve, reject) => reject(value));
    }
  }
  finally(onDone) {
    const Promise = this.constructor;
    return this.then(
      value => Promise.resolve(onDone()).then(() => value),
      reason => Promise.resolve(onDone()).then(() => { throw reason })
    );
  }
}
```
