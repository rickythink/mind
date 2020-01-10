# vue 数组变异

什么是变异数组方法？

变异数组方法即保持数组方法原有功能不变的前提下对其进行功能拓展，在 `Vue` 中这个所谓的功能拓展就是添加响应式功能。

将普通的数组变为变异数组的方法分为两步：

1. 数组劫持
2. 功能拓展

## 数组劫持

{% code title="src/core/observer/index.js" %}
```javascript
if (Array.isArray(value)) {
    // 能力检测
    const augment = hasProto
    ? protoAugment
    : copyAugment
    // 通过能力检测的结果选择不同方式进行数组劫持
    augment(value, arrayMethods, arrayKeys)
    // 对数组的响应式处理
    this.observeArray(value)
}
```
{% endcode %}

其中

{% code title="hasProto" %}
```text
export const hasProto = '__proto__' in {}
```
{% endcode %}

```javascript
function protoAugment (target, src: Object, keys: any) {
  /* eslint-disable no-proto */
  target.__proto__ = src
  /* eslint-enable no-proto */
}

function copyAugment (target: Object, src: Object, keys: Array<string>) {
  for (let i = 0, l = keys.length; i < l; i++) {
    const key = keys[i]
    // Object.defineProperty的封装
    def(target, key, src[key])
  }
}
```

根据根据能力检测结果选择不同的数组劫持方法，如果浏览器支持隐式原型，则调用 `protoAugment` 函数作为数组劫持的方法\( 原型链继承），反之则使用 `copyAugment` .`copyAugment` 函数就很清晰了，将所有变异数组中的方法，直接定义在数组实例本身，相当于变相的实现了数组的劫持。

## 功能拓展

```javascript
import { def } from '../util/index'

// 缓存数组原型
const arrayProto = Array.prototype
// 实现 arrayMethods.__proto__ === Array.prototype
export const arrayMethods = Object.create(arrayProto)

// 需要进行功能拓展的方法
const methodsToPatch = [
  'push',
  'pop',
  'shift',
  'unshift',
  'splice',
  'sort',
  'reverse'
]

/**
 * Intercept mutating methods and emit events
 */
methodsToPatch.forEach(function (method) {
  // cache original method
  // 缓存原生数组方法
  const original = arrayProto[method]
  // 在变异数组中定义功能拓展方法
  def(arrayMethods, method, function mutator (...args) {
    // 执行并缓存原生数组方法的执行结果
    const result = original.apply(this, args)
    // 响应式处理
    const ob = this.__ob__
    let inserted
    switch (method) {
      case 'push':
      case 'unshift':
        inserted = args
        break
      case 'splice':
        inserted = args.slice(2)
        break
    }
    if (inserted) ob.observeArray(inserted)
    // notify change
    ob.dep.notify()
    // 返回原生数组方法的执行结果
    return result
  })
})
```

可以发现，源码在实现的方式上，和我在数组变异思路中采用的方法一致，只不过在其中添加了响应式的处理。

#### 总结

`Vue` 的变异数组从本质上是来说是一种**装饰器模式**，通过学习它的原理，我们在实际工作中可以轻松处理这类保持原有功能不变的前提下对其进行功能拓展的需求。

