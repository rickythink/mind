# 内存泄露

\
**避免内存泄漏的方法**\


1. 少用全局变量，避免意外产生全局变量
2. 使用闭包要及时注意，有Dom元素的引用要及时清理。
3. 计时器里的回调没用的时候要记得销毁。
4. 为了避免疏忽导致的遗忘，我们可以使用 `WeakSet` 和 `WeakMap`结构，它们对于值的引用都是不计入垃圾回收机制的，表示这是弱引用。举个例子：

```javascript
const wm = new WeakMap();const element = document.getElementById('example');

wm.set(element, 'some information');
wm.get(element) // "some information"
```

这种情况下，一旦消除对该节点的引用，它占用的内存就会被垃圾回收机制释放。Weakmap 保存的这个键值对，也会自动消失。

基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 WeakMap。
