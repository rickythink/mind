# 后序遍历

后序遍历的顺序是`左右根`

## 递归实现

```javascript
const postOrder = function (root, array = []) {
  if (root) {
    postOrder(root.left, array);
    postOrder(root.right, array);
    array.push(root.val);
  }
  return array;
};
```

## 非递归实现

* 取跟节点为目标节点，开始遍历
* 1.左孩子入栈 -&gt; 直至左孩子为空的节点
* 2.栈顶节点的右节点为空或右节点被访问过 -&gt; 节点出栈并访问他，将节点标记为已访问
* 3.栈顶节点的右节点不为空且未被访问，以右孩子为目标节点，再依次执行1、2、3

```javascript
const postOrder = function (root) {
  const result = [];
  const stack = [];
  let last = null; // 标记上一个访问的节点
  let current = root;
  while (current || stack.length > 0) {
    while (current) {
      stack.push(current);
      current = current.left;
    }
    current = stack[stack.length - 1];
    if (!current.right || current.right == last) {
      current = stack.pop();
      result.push(current.val);
      last = current;
      current = null; // 继续弹栈
    } else {
      current = current.right;
    }
  }
  return result;
}
```

## 更好的实现，双栈

```javascript
/**
 * 后序遍历
 * 非递归方式
 *
 * 双栈，一个栈用来按顺序记录左右孩子，一个栈用来弹栈输出结果
 * @param {树} root
 */
export function backNoRecursive(root) {
  const result = [];
  const stack1 = [];
  const stack2 = [];
  let current = root;
  stack1.push(current);
  while (current || stack1.length > 0) {
    current = stack1.pop();
    stack2.push(current);
    if (current.left) stack1.push(current.left);
    if (current.right) stack1.push(current.right);
  }
  while (stack2.length > 0) {
    result.push(stack2.pop());
  }
  return result;
}
```

