# Object.defineProperty与Proxy区别

> [https://www.infoq.cn/article/sPCMAcrdAZQfmLbGJeGr](https://www.infoq.cn/article/sPCMAcrdAZQfmLbGJeGr)

上面已经知道 `Object.defineProperty` 对数组和对象的表现是一致的，那么它和 Proxy 对比存在哪些优缺点呢？

{% hint style="info" %}
社区误区纠正，事实上，`Object.defineProperty` 本身是可以监控到数组下标的变化的
{% endhint %}

**1. `Object.defineProperty`只能劫持对象的属性，而 Proxy 是直接代理对象。**

由于 `Object.defineProperty` 只能对属性进行劫持，需要遍历对象的每个属性，如果属性值也是对象，则需要深度遍历。而 Proxy 直接代理对象，不需要遍历操作。

**2. `Object.defineProperty`对新增属性需要手动进行 Observe。**

由于 `Object.defineProperty`劫持的是对象的属性，所以新增属性时，需要重新遍历对象，对其新增属性再使用 `Object.defineProperty` 进行劫持。

也正是因为这个原因，使用 Vue 给 data 中的数组或对象新增属性时，需要使用 vm.$set 才能保证新增的属性也是响应式的。

## 总结

1. `Object.defineProperty` 并非不能监控数组下标的变化，Vue2.x 中无法通过数组索引来实现响应式数据的自动更新是 Vue 本身的设计导致的，不是 defineProperty 的锅。
2. `Object.defineProperty` 和 `Proxy` 本质差别是，`defineProperty` 只能对属性进行劫持，所以出现了需要递归遍历，新增属性需要手动 `Observe` 的问题。
3. `Proxy` 作为新标准，浏览器厂商势必会对其进行持续优化，但它的兼容性也是块硬伤，并且目前还没有完整的 polyfill 方案。

