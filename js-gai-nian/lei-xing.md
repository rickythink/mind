# 类型

对象是JavaScript的基础。在JavaScript中一共有六种主要类型（术语是“语言类型”）：

* string
* number
* boolean
* null
* undefined
* object

注意， 简单基本 类型（ string 、 boolean 、 number 、 null 和 undefined ）本身并不是对象。 null 有时会被当作一种对象类型， 但是这其实只是语言本身的一个 bug， 即对 null 执行 typeof null 时会返回字符串 "object" 。 1 实际上， null 本身是基本类型。

有一种常见的错误说法是“JavaScript 中万物皆是对象”，这显然是错误的。

**更新**

最新的 ECMAScript 标准定义了 8 种数据类型:

* 7 种原始类型:
  * Boolean
  * Null
  * Undefined
  * Number
  * BigInt
  * String
  * Symbol 
* 和 Object

函数就是对象的一个子类型（从技术角度来说就是“可调用的对象”）。 JavaScript 中的函 数是“一等公民”，因为它们本质上和普通的对象一样（只是可以调用），所以可以像操作 其他对象一样操作函数（比如当作另一个函数的参数）。

数组也是对象的一种类型，具备一些额外的行为。数组中内容的组织方式比一般的对象要 稍微复杂一些。

### 存储方式

基础原始类型存在栈\(stack\)中，对象类型存在堆\(Heap\)中

![](../.gitbook/assets/image%20%2848%29.png)

### 数组

在JavaScript中，类型相同的数组是连续内存，类型不同的数组是非连续内存。

> [https://juejin.im/post/5d2832a5f265da1bc07e669e](https://juejin.im/post/5d2832a5f265da1bc07e669e)

