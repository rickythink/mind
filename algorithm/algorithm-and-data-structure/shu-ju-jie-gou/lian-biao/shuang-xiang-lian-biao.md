# 双向链表

双向链表中节点除了有 `next` 指向下一个节点，还有`prev`指向前一个节点。双向链表的优点在于能够从头到尾迭代，也能够从尾到头迭代。如果从头到尾遍历到中间位置的时候，想反向从尾到头进行遍历，也是可以办到的。

双向链表虽然比单向链表占了更多的内存，但是双向链表最大的好处是删除给定指针操作时不用再遍历一遍找到其 `prev` 指向的节点，所以此时的删除操作单向链表时间复杂度是 `O(n)`，双向链表的时间复杂度是 `O(1)`。\


![](<../../../../.gitbook/assets/image (123).png>)

```javascript
const DoubleLinkedList = (function(){
  let Node = function (element) {
    this.element = element
    this.prev = this.next = null
  }
  class DoubleLinkedList {
    constructor () {
      this.head = this.tail = null
      this.length = 0
    }
    append (element) {
      let newNode = new Node(element)
      if (!this.head) {
        this.head = this.tail = newNode
      } else {
        let current = this.head
        while (current) {
          current = current.next
        }
        current = this.tail
        current.next = this.tail = newNode
        newNode.prev = current
      }
    }
    insert (position, element) {
      if (position < 0 || position > this.length) {
        return false
      }
      let newNode = new Node(element)
      let previous, current = this.head, index = 0
      if (position === 0) {
        if (!this.head) {
          this.head = this.tail = newNode
        } else {
          newNode.next = current
          current.prev = newNode
          this.head = newNode
        }
      } else if (position === this.length) {
        this.tail.next = newNode
        newNode.prev = this.tail
        this.tail = newNode
      } else {
        while (index++ < position) {
          previous = current
          current = newNode
        }
        previous.next = newNode
        newNode.prev = previous
        newNode.next = current
        current.prev = newNode
      }
      this.length++
      return true
    }
    removeAt (position) {
      if (position < 0 || position >= this.length) {
        return false
      }
      let previous, current = this.head, index = 0
      if (position === 0) {
        this.head = current.next
        if (this.length === 1) {
          this.tail = null // 若只有一项，则 current.next 为 null ，所以只需要将尾部设为 null
        } else {
          this.head.prev = null
        }
      } else if (position === this.length - 1) {
        current = this.tail
        this.tail = current.prev
        this.tail.next = null
      } else {
        while (index++ < positon) {
          previous = current
          current = current.next
        }
        previous.next = current.next
        current.next.prev = previous
      }
      this.length--
      return current.element
    }
    // 删除指定节点
    removeByNode (node) {
      if (!node.prev) {
        this.head = node.next
        this.head.prev = null
        return
      }
      if (!node.next) {
        return
      }
      let prev = node.prev
      let next = node.next
      prev.next = next
      next.prev = prev
    }
    // 其他方法实现和单向链表相同
  }
  return DoubleLinkedList
})
```
