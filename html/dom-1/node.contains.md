---
description: 判断node的包含关系
---

# node.contains

## 简单示例

```javascript
// 判断元素是否body元素且是否是body的子孙元素.
function isInPage(node) {
  return (node === document.body) ? false : document.body.contains(node);
}
```

## 场景

比较常见的是**弹出菜单**的关闭, 通过`contains`判断点击元素是否是菜单本身或在菜单内, 如果不在其内那么表示要关闭菜单

