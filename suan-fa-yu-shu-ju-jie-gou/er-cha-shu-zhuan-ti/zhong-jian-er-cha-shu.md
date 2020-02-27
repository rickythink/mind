# 重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如输入前序遍历序列`{1,2,4,7,3,5,6,8}`和中序遍历序列`{4,7,2,1,5,3,8,6}`，则重建二叉树并返回。

## 思路

* 前序遍历：根节点 + 左子树前序遍历 + 右子树前序遍历（根左右）
* 中序遍历：左子树中序遍历 + 根节点 + 右字数中序遍历（左根右）
* 后序遍历：左子树后序遍历 + 右子树后序遍历 + 根节点（左右根）

根据上面的规律：

* 前序遍历找到根结点`root`
* 找到`root`在中序遍历的位置 -&gt; 左子树的长度和右子树的长度
* 截取左子树的中序遍历、右子树的中序遍历
* 截取左子树的前序遍历、右子树的前序遍历
* 递归重建二叉树

![](../../.gitbook/assets/image%20%28169%29.png)

```javascript
// pre 是前序数组 
// vin 是中序数组
function reConstruct(pre, vin) {
    if(pre.length === 0){
        return null;
    }
    if(pre.length === 1){
        return new TreeNode(pre[0]);
    }
    const value = pre[0];
    const index = vin.indexOf(value);
    const vinLeft = vin.slice(0,index);
    const vinRight = vin.slice(index+1);
    const preLeft = pre.slice(1,index+1);
    const preRight = pre.slice(index+1);
    const node = new TreeNode(value);
    node.left = reConstruct(preLeft, vinLeft);
    node.right = reConstruct(preRight, vinRight);
    return node;
}
```

