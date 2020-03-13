# 单链表

顾名思义，单向链表是指只有 `next` 没有 `prev` 的链表数据结构。单向链表详细实现如下

![](../../../../.gitbook/assets/image%20%2898%29.png)

```javascript
const LinkedList = (function() {
  const Node = function (element) { // 构造新节点
    this.element = element
    this.next = null
  }
  class LinkedList {
    constructor () { // 初始化时候生成头节点 head 和链表长度 length
      this.head = null
      this.length = 0
    }
    append (element) { // 从尾部添加节点
      let newNode = new Node(element)
      if (!this.head) { // 没有头节点，就将新节点设为头节点
        this.head = newNode
      } else { // 存在头节点，就在链表尾部添加新节点
        let current = this.head
        while (current.next) { // 遍历找到链表尾部
          current = current.next
        }
        current.next = newNode
      }
      this.length++
    }
    insert (position, element) { // 按位置插入节点
      if (position < 0 || position > this.length) { // 边界判断
        return false
      }
      let newNode = new Node(element)
      if (position === 0) { // 往链表首部添加新节点
        newNode.next = this.head
        this.head = newNode
      } else { // 非链表首部添加新节点
        let index = 0, previous , current = this.head // index 索引判断是否是当前 position
        while (index++ < position) { // 如果 index 小于 position，递增并将变量移动到下一个节点
          previous = current
          current = current.next
        }
        newNode.next = current
        previous.next = newNode
      }
      this.length++
      return true
    }
    removeAt (position) { // 按照位置删除节点
      if (position < 0 || position > this.length) {
        return null
      }
      let index = 0, previous , current = this.head
      if (position === 0) {
        this.head = current.next
      } else {
        while (index++ < position) {
          previous = current
          current = current.next
        }
        previous.next = current.next
      }
      this.length--
      return current.element
    }
    toString (symbol) { // 将链表的值字符串化
      let current = this.head, str = ''
      while (current) {
        str += current.element
        current = current.next
        if (current) {
          str += symbol ? symbol : ','
        }
      }
      return str
    }
    indexOf (element) { // 找到值第一次出现的位置
      let current = this.head, index = 0
      while (current) {
        if (current.element === element) {
          return index
        }
        current = current.next
        index++
      }
      return -1
    }
    find (element) { // 找到第一次出现该值的节点
      let current = this.head
      while (current) {
        if (current.element === element) {
          return current
        }
        current = current.next
      }
      return false
    }
    isEmpty () { // 判断链表是否为空
      return this.length === 0
    }
    size () { // 返回链表长度
      return this.length
    }
    getHead () { // 获取链表头节点
      return this.head
    }
  }
  return LinkedList
})()

作者：幻灵尔依
链接：https://juejin.im/post/5d65ffa051882518e0056241
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

