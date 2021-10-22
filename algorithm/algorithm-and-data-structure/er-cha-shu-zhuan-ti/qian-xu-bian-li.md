# 前序遍历

前序遍历是`根左右`&#x20;

## 递归实现

```javascript
function preOrder(root, array = []) {
  if (root) {
    //根左右
    array.push(root.val);
    preOrder(root.left, array);
    preOrder(root.right, array);
  }
  return array;
}
```

## 非递归实现

* 取根节点为目标节点，开始遍历
* 1.访问目标节点
* 2.左孩子入栈 -> 直至左孩子为空的节点
* 3.节点出栈，以右孩子为目标节点，再依次执行1、2、3

```javascript
 const preOrder = function (root) {
    const result = [];
    const stack = [];
    let current = root;
    while (current || stack.length > 0) {
      while (current) {
        result.push(current.val);
        stack.push(current);
        current = current.left;
      }
      current = stack.pop();
      current = current.right;
    }
    return result;
  };
```
