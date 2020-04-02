# 集合

集合是由一组无序且唯一的元素组成的数据结构，由于不会重复，所以集合中键就是值。

```javascript
class MySet extends Set {
  constructor (...args) {
    super(...args)
  }
  union (otherSet) { // 并集
    return new MySet([...this, ...otherSet])
  }
  intersection (otherSet) { // 交集
    // return new Set([...this].filter(x => otherSet.has(x))) // 标记 aa
    let newSet = new MySet()
    for (let a of this) {
      if (otherSet.has(a)) {
        newSet.add(a)
      }
    }
    return newSet
  }
  difference (otherSet) { // 差集
    // return new Set([...this].filter(x => !otherSet.has(x))) // 标记 bb
    let newSet = new MySet()
    for (let x of this) {
      if (!otherSet.has(x)) {
        newSet.add(x)
      }
    }
    return newSet
  }
  isSubOf (otherSet) { // 子集
    if (this.size > otherSet.size) {
      return false
    } else {
      // return [...this].every(item => otherSet.has(item)) // 标记 cc
      for (let x of this) {
        if (!otherSet.has(item)) {
          return false
        }
      }
      return true
    }
  }
}
var a = new MySet([1, 2, 3, 4])
var b = new MySet([3, 4, 5, 6])
var c = new MySet([1,2])
a.intersection(b) // Set(2) {3, 4}
a.difference(b) // Set(2) {1, 2}
a.union(b) // Set(6) {1, 2, 3, 4, 5, 6}
c.isSubOf(a) // true
c.isSubOf(b) // false
```

上面代码中标记的“aa bb cc”三处一行代码就可以实现其功能，但是其时间复杂度为 O\(n²\)，相比之下其下面的代码时间复杂度为 O\(n\)。因为 `[...this]` 先遍历集合中所有元素转为数组，然后用`every`遍历数组中的元素。

