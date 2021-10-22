---
description: 弱引用集合
---

# WeakMap

WeakMao 与 Map 结构类似，用于生成键值对的集合。但是具备与 Map 的如下区别：

### WeakMap 只接受对象作为键名( null 也不行 ), 不接受其他类型的值作为键名

```javascript
const map = new WeakMap()
map.set(1, 2)
// TypeError: 1 is not an object!
map.set(Symbol(), 2)
// TypeError: Invalid value used as weak map key
map.set(null, 2)
// TypeError: Invalid value used as weak map key
```

### 没有遍历

WeakMap 没有 key(), values(), entries() 方法，也没有 size() 属性

### 无法清空

WeakMap 不支持 clear() 方法



WeakMap 仅有 4个方法可用：get(), set(), has() 和 delete()
