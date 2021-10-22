# 中序遍历

中序遍历是`左根右`&#x20;

## 递归版本

```javascript
function inorder(root, array = []) {
  if (root) {
    inorder(root.left, array);
    array.push(root.val);
    inorder(root.right, array);
  }
  return array;
};
```

## 非递归实现

* 取跟节点为目标节点，开始遍历
* 1.左孩子入栈 -> 直至左孩子为空的节点
* 2.节点出栈 -> 访问该节点
* 3.以右孩子为目标节点，再依次执行1、2、3

```javascript
const inOrder = function (root) {
    const result = [];
    const stack = [];
    let current = root;
    while (current || stack.length > 0) {
      while (current) {
        stack.push(current);
        current = current.left;
      }
      current = stack.pop();
      result.push(current.val);
      current = current.right;
    }
    return result;
  };
```
