# LRU



Design and implement a data structure for Least Recently Used \(LRU\) cache. It should support the following operations: `get` and `put`.

`get(key)` - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
`put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

Follow up:  
Could you do both operations in _O\(1\)_ time complexity?

Example:

```text
LRUCache cache = new LRUCache( 2 /* capacity */ );
cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

#### JavaScript解法 <a id="JavaScript&#x89E3;&#x6CD5;"></a>

分析一下题目的意思，这道题是让我们设计一个LRU（最近最少使用）缓存。

* 调用`get`方法时查找缓存中对应`key`的值。
* 调用`put`时可以加入或者更新键值。
* 每次调用`get`和`put`的时候，都会刷新键值为“最近使用”；而且插入新值的时候如果超出`capacity`最旧的键值会被舍弃。
* Follow up 中要求`get`和`put`的时间复杂度都是O\(1\)

既然要实现插入和查找都是O\(1\)复杂度的键值存取，首先直接会想到HashMap。但要同时实现LRU机制，单单一个无序的HashMap是不够的。这里我想到了Java里面的LinkedHashMap，它可以保留hashmap的插入或者访问顺序。那么在JavaScript中怎么实现呢？可以用一个链表来保存键值对的顺序，对于`get`和`put`已有的值，都是去掉节点再加到结尾；注意如果超出了capacity，就直接去掉代表最旧节点的链表head就行了。为了方便快速在链表中找到已有的值，可以直接在hashmap里面保存链表节点的对象。

实现起来再到修修改改到通过OJ还是费了很多工夫的。

```javascript
class ListNode {
  constructor(key, value) {
    this.key = key;
    this.val = value;
    this.prev = null;
    this.next = null;
  }
}
class LRUCache {
  constructor(capacity) {
    this.cap = capacity;
    this.map = {};
    // 实际保存的键值对数量
    this.size = 0;
    // 代表最旧的结点
    this.head = null;
    // 代表最新的结点
    this.tail = null;
  }
  get(key) {
    if (!(key in this.map)) {
      return -1;
    }
    let node = this.map[key];
    this.put(node.key, node.val);
    return node.val;
  }
  addToTail(node) {
    if (this.tail) {
      this.tail.next = node;
      node.prev = this.tail;
      this.tail = node;
    } else {
      this.tail = this.head = node;
    }
  }
  remove(node) {
    if (node.prev) {
      node.prev.next = node.next;
    } else {
      this.head = this.head.next;
    }
    if (node.next) {
      node.next.prev = node.prev;
    } else {
      this.tail = this.tail.prev;
    }
    node.prev = node.next = null;
  }
  put(key, value) {
    if (key in this.map) {
      let node = this.map[key];
      node.val = value;
      this.remove(node);
      this.addToTail(node);
    } else {
      let node = new ListNode(key, value);
      this.addToTail(node);
      this.map[key] = node;
      this.size++;
    }
    // 超出capacity，淘汰最老的
    if (this.size > this.cap) {
      let key = this.head.key;
      this.remove(this.head);
      delete this.map[key];
      this.size--;
    }
  }
}
```

#### ES6 Map的简单解法 <a id="ES6-Map&#x7684;&#x7B80;&#x5355;&#x89E3;&#x6CD5;"></a>

这种解法是之后看到别人的做法：[ES6 Javascript, O\(1\), one Map, fewest lines of code](https://leetcode.com/problems/lru-cache/discuss/134851/ES6-Javascript-O%281)-one-Map-fewest-lines-of-code/155303\)

划重点，ES6的Map遍历顺序就是插入顺序！所以这种解法非常简洁，学习一下。

```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.map = new Map();
  }
  get(key) {
    let val = this.map.get(key);
    if (typeof val === 'undefined') { return -1 }
    this.map.delete(key);
    this.map.set(key, val);
    return val;
  }
  put(key, value) {
    if (this.map.has(key)) { this.map.delete(key) }
    this.map.set(key, value);
    let keys = this.map.keys();
    while (this.map.size > this.capacity) { this.map.delete(keys.next().value) }
  }
}
```

