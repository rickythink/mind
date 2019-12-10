# Object.defineProperty与Proxy区别

1. Object.defineProperty无法处理数组的push、pop、shift、unshift、splice、sort、reverse相应监听
2.  `Object.defineProperty` 劫持的是对象的属性，所以新增属性时，需要重新遍历对象，对其新增属性再使用 `Object.defineProperty` 进行劫持。

   也正是因为这个原因，使用vue给 `data` 中的数组或对象新增属性时，需要使用 `vm.$set` 才能保证新增的属性也是响应式的。

3. proxy兼容性比较差.

