# 字典

字典就是根据键来查询值的数据结构，也就是我们常说的“键值对”。

字典数据结构在 es6 的实现中为 `Map` 。 `Map` 和 `Object` 一样是由键值对组成的，键值都不能重复。但是不同之处在于 `Object` 的键只能是字符串，且是无序的；而 `Map` 的键可以是任何类型的值，包括对象，且是有序的。

```javascript
let o = { 2: 2, 1: 1 }
Object.keys(o) // ['1', '2']  键只能是无序的字符串
let map = new Map()
map.set(o, 1)
map.set(2, 2)
for (let key of map) console.log(key) // [{…}, 1] [2, 2]  键可以是任意类型，且根据添加的顺序遍历
```

在栈和队列的实现中我们用到了 `WeakMap` 类，其实 `WeakMap WeakSet` 类几乎和 `Map Set` 类一样，只有以下少许不同：

* 只能用对象作为键
* 键是弱引用类型的
* `WeakMap WeakSet` 没有遍历器，所以不能调用 `keys()、values()、entries()`方法，也没有 `size` 属性和 `clear()` 方法，最有四个增删改查方法 `get()、set()、has()、delete()`

