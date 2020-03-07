# Vue diff原理

## 灵感来源

{% embed url="https://github.com/snabbdom/snabbdom" %}

## Vitrual DOM与真实DOM

真实DOM

```markup
<div>
    <p>123</p>
</div>
```

Virtual DOM（伪代码\)

```javascript
var Vnode = {
  tag: "div",
  children: [{ tag: "p", text: "123" }]
};
```

## 详解

### 仅在同层比较，不会跨层级比较

![](../../.gitbook/assets/image%20%28148%29.png)

## 流程图

![](../../.gitbook/assets/image%20%28163%29.png)

## 代码部分

### patch 

```javascript
function patch(oldVnode, vnode) {
  // some code
  if (sameVnode(oldVnode, vnode)) {
    patchVnode(oldVnode, vnode);
  } else {
    const oEl = oldVnode.el; // 当前oldVnode对应的真实元素节点
    let parentEle = api.parentNode(oEl); // 父元素
    createEle(vnode); // 根据Vnode生成新元素
    if (parentEle !== null) {
      api.insertBefore(parentEle, vnode.el, api.nextSibling(oEl)); // 将新元素添加进父元素
      api.removeChild(parentEle, oldVnode.el); // 移除以前的旧元素节点
      oldVnode = null;
    }
  }
  // some code
  return vnode;
}
```

patch函数主要进行了两个分支流程

1. 判断两节点是否是一样的，如果是则执行 `patchVnode`
2. 不一样则用 `Vnode` 替换 `oldVnode`

### patchVnode

```javascript
patchVnode (oldVnode, vnode) {
    const el = vnode.el = oldVnode.el
    let i, oldCh = oldVnode.children, ch = vnode.children
    if (oldVnode === vnode) return
    if (oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text) {
        api.setTextContent(el, vnode.text)
    }else {
        updateEle(el, vnode, oldVnode)
    	if (oldCh && ch && oldCh !== ch) {
            updateChildren(el, oldCh, ch)
    	}else if (ch){
            createEle(vnode) //create el's children dom
    	}else if (oldCh){
            api.removeChildren(el)
    	}
    }
}
```

关键流程

1. 找到对应的真实 dom，称为 `el`
2. 判断 `Vnode` 和 `oldVnode` 是否指向同一个对象，如果是，那么直接 `return`
3. 如果他们都有文本节点并且不相等，那么将 `el` 的文本节点设置为 `Vnode` 的文本节点
4. 如果 `oldVnode` 有子节点而 `Vnode` 没有，则删除 `el` 的子节点
5. 如果 `oldVnode` 没有子节点而 `Vnode` 有，则将 `Vnode` 的子节点真实化之后添加到 `el`
6. 如果两者都有子节点，则执行 `updateChildren` 函数比较子节点，这一步很重要

### updateChildern\(核心\)

![&#x7C89;&#x7EA2;&#x8272;&#x7684;&#x90E8;&#x5206;&#x4E3A; oldCh \| &#x9EC4;&#x8272;&#x90E8;&#x5206;&#x4E3A; vCh](../../.gitbook/assets/image%20%28125%29.png)

我们将它们取出来并分别用 s 和 e 指针指向它们的头 `child` 和尾 `child`

![](../../.gitbook/assets/image%20%2863%29.png)



现在分别对 **`oldS`、`oldE`、`S`、`E` 两两做 `sameVnode` 比较，有四种比较方式，当其中两个能匹配上那么真实 `dom` 中的相应节点会移到 Vnode 相应的位置**，这句话有点绕，打个比方

* 如果是 `oldS` 和 `E` 匹配上了，那么真实 `dom` 中的第一个节点会移到最后
* 如果是 `oldE` 和 `S` 匹配上了，那么真实 `dom` 中的最后一个节点会移到最前，匹配上的两个指针向中间移动
* 如果四种匹配没有一对是成功的，分为两种情况
  * 如果新旧子节点都存在 `key`，那么会根据 `oldChild` 的 `key` 生成一张 `hash` 表，用 `S` 的 `key` 与 `hash` 表做匹配，匹配成功就判断 `S` 和匹配节点是否为 `sameNode`，如果是，就在真实 `dom` 中将成功的节点移到最前面，否则，将 `S` 生成对应的节点插入到 `dom` 中对应的 `oldS` 位置，`oldS` 和 `S` 指针向中间移动。
  * 如果没有 `key`,则直接将 `S` 生成新的节点插入真实 `DOM`（ps：**这下可以解释为什么 `v-for` 的时候需要设置 `key` 了，如果没有 `key` 那么就只会做四种匹配，就算指针中间有可复用的节点都不能被复用了**）

再配个图（假设下图中的所有节点都是有 `key` 的，且 `key` 为自身的值）  


![](../../.gitbook/assets/image%20%28123%29.png)

* 1.第一步

```javascript
oldS = a, oldE = d；S = a, E = b;
```

`oldS` 和 `S` 匹配，则将 `dom` 中的 `a` 节点放到第一个，已经是第一个了就不管了，此时 `dom` 的位置为：a b d

* 2. 第二步

```javascript
oldS = b, oldE = d；S = c, E = b
```

`oldS` 和 `E` 匹配，就将原本的 `b` 节点移动到最后，因为 `E` 是最后一个节点，他们位置要一致，这就是上面说的：**当其中两个能匹配上那么真实 `dom` 中的相应节点会移到 Vnode 相应的位置**，此时 dom 的位置为：a d b

* 3. 第三步

`oldE` 和 `E` 匹配，位置不变此时 `dom` 的位置为：a d b

* 4. 第四步

遍历结束，说明 `oldCh` 先遍历完。就将剩余的 `vCh` 节点根据自己的的 `index` 插入到真实 `dom` 中去，此时 `dom` 位置为：a c d b  
  
这个匹配过程的结束有两个条件：

* `oldS > oldE` 表示 `oldCh` 先遍历完，那么就将多余的 `vCh` 根据 `index` 添加到 `dom` 中去（如上图）
* `S > E` 表示 `vCh` 先遍历完，那么就在真实 `dom` 中将区间为`[oldS, oldE]`的多余节点删掉

![](../../.gitbook/assets/image%20%28180%29.png)

