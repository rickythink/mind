# 求二叉树的遍历

给定一棵二叉树的前序遍历和中序遍历，求其后序遍历

输入描述:

两个字符串，其长度n均小于等于26。 第一行为前序遍历，第二行为中序遍历。 二叉树中的结点名称以大写字母表示：A，B，C....最多26个结点。

输出描述:

输入样例可能有多组，对于每组测试样例， 输出一行，为后序遍历的字符串。

样例：

```text
输入
ABC
BAC
FDXEAG
XDEFAG

输出
BCA
XEDGAF
```

### 思路 <a id="&#x601D;&#x8DEF;-2"></a>

和上面题目的思路基本相同

* 前序遍历找到根结点`root`
* 找到`root`在中序遍历的位置 -&gt; 左子树的长度和右子树的长度
* 截取左子树的中序遍历、右子树的中序遍历
* 截取左子树的前序遍历、右子树的前序遍历
* 递归拼接二叉树的后序遍历

## 实现

```javascript
let pre;
let vin;
 
while((pre = readline())!=null){
  vin = readline();
  print(getHRD(pre,vin));
}
 
function getPostOrder(pre, vin) {
  if (!pre) {
    return '';
  }
  if (pre.length === 1) {
    return pre;
  }
  const head = pre[0];
  const splitIndex = vin.indexOf(head);
  const vinLeft = vin.substring(0, splitIndex);
  const vinRight = vin.substring(splitIndex + 1);
  const preLeft = pre.substring(1, splitIndex + 1);
  const preRight = pre.substring(splitIndex + 1);
  return getPostOrder(preLeft, vinLeft) + getPostOrder(preRight, vinRight) + head;
}
```

