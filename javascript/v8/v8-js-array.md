# JS 数组

## 简介

数组、链表、栈、队列都是线性表，它表示的结构都是一段线性的结构，与之对应的就是非线性表，例如树、图、堆等，它表示的结构都非线性。

本节主要介绍 JavaScript 数组，在开始本章节前，思考一个问题：

我们知道在 JavaScript 中，可以在数组中保存不同类型值，并且数组可以动态增长，不像其它语言，例如 C，创建的时候要决定数组的大小，如果数组满了，就要重新申请内存空间。这是为什么喃？

本节从 Chrome v8 源码角度回答这个问题，分为四个方面：

* 数组基础入门
* JavaScript 中，数组为什么可以保存不同类型？
* JavaScript 中，数组是如何存储的喃？
* JavaScript 中，数组的动态扩容与减容

## 数组（基础）

一种最基础的数据结构，每种编程语言都有，它编号从 0 开始，代表一组连续的储存结构，用来储存同一种类型的数据。

```javascript
let arr = [1, 2, 3]
```

它的这种特定的存储结构（连续存储空间存储同一类型数据）决定了：

**优点**

* 随机访问：可以通过下标随机访问数组中的任意位置上的数据

**缺点**

* 对数据的删除和插入不是很友好

**查找：** 根据下标随机访问的时间复杂度为 O\(1\)；

**插入或删除：** 时间复杂度为 O\(n\)；

在 JavaScript 中的数组几乎是万能的，它不光可以作为一个普通的数组使用，可以作为栈或队列使用。

数组：

```javascript
let array = [1, 2, 3]
```

栈：

```javascript
let stack = [1, 2, 3]
// 进栈
stack.push(4)
// 出栈
stcak.pop()
```

队列：

```javascript
let queue = [1, 2, 3]
// 进队
queue.push(4)
// 出队
queue.shift()
```

## JS中数组可以保存不同类型

看一下 Chrome v8 源码：

```javascript
// The JSArray describes JavaScript Arrays
//  Such an array can be in one of two modes:
//    - fast, backing storage is a FixedArray and length <= elements.length();
//       Please note: push and pop can be used to grow and shrink the array.
//    - slow, backing storage is a HashTable with numbers as keys.
class JSArray: public JSObject {
 public:
  // [length]: The length property.
  DECL_ACCESSORS(length, Object)
    
  // ...
   
  // Number of element slots to pre-allocate for an empty array.
  static const int kPreallocatedArrayElements = 4;
};
```

我们可以看到 `JSArray` 是继承自 `JSObject` 的，所以在 JavaScript 中，数组可以是一个特殊的对象，内部也是以 key-value 形式存储数据，所以 JavaScript 中的数组可以存放不同类型的值。  




