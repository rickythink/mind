# 泛型变量

我们现在假设有这样的需求，我们的函数接受一个数组，如何把数组的长度打印出来，最后返回这个数组，我们应该如何定义？

我们当然得运用上刚才学的泛型：

```javascript
function getArrayLength<T>(arg: T): T {
  console.log(arg.length) // 类型“T”上不存在属性“length”
  return arg
}
```

糟糕，在编写过程中报错了，编译器告诉我们「他们不知道类型T上有没有 length 这个属性」。

所以我们得想办法告诉编译器，这个类型 T 是有 length 属性的，不然在编译器眼里，这个 T 是可以代表任何类型的。

我们已经明确知道要传入的是一个数组了，为什么不这样声明呢: `Array<T>`？

反正传入的类型不管如何，这起码是数组是可以确定的，在这里泛型变量 T 当做类型的一部分使用，而不是整个类型，增加了灵活性。

```javascript
function getArrayLength<T>(arg: Array<T>) {
  console.log((arg as Array<any>).length) // ok
  return arg
}
```



