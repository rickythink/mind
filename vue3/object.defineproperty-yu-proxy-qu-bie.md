# Object.defineProperty与Proxy区别

> [https://www.infoq.cn/article/sPCMAcrdAZQfmLbGJeGr](https://www.infoq.cn/article/sPCMAcrdAZQfmLbGJeGr)

上面已经知道 `Object.defineProperty` 对数组和对象的表现是一致的，那么它和 Proxy 对比存在哪些优缺点呢？

**1. `Object.defineProperty`只能劫持对象的属性，而 Proxy 是直接代理对象。**

由于 `Object.defineProperty` 只能对属性进行劫持，需要遍历对象的每个属性，如果属性值也是对象，则需要深度遍历。而 Proxy 直接代理对象，不需要遍历操作。

**2. `Object.defineProperty`对新增属性需要手动进行 Observe。**

由于 `Object.defineProperty`劫持的是对象的属性，所以新增属性时，需要重新遍历对象，对其新增属性再使用 `Object.defineProperty` 进行劫持。

也正是因为这个原因，使用 Vue 给 data 中的数组或对象新增属性时，需要使用 vm.$set 才能保证新增的属性也是响应式的。

