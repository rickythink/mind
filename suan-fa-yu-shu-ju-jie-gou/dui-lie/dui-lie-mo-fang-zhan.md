# 队列模仿栈

使用队列实现栈的下列操作：

* push\(x\) -- 元素 x 入栈 
* pop\(\) -- 移除栈顶元素 
* top\(\) -- 获取栈顶元素 
* empty\(\) -- 返回栈是否为空

```javascript
/**
 * Initialize your data structure here.
 */
var MyStack = function() {
    this.list = [];
    this.topEl = null;
};

/**
 * Push element x onto stack. 
 * @param {number} x
 * @return {void}
 */
MyStack.prototype.push = function(x) {
    this.list.push(x);
    this.topEl = x;
};

/**
 * Removes the element on top of the stack and returns that element.
 * @return {number}
 */
MyStack.prototype.pop = function() {
    let temp = [];
    while(this.list.length > 1) {
            let s = this.list.shift();
            temp.push(s);
            this.topEl = s;
    }
    let topEl = this.list.shift();
    this.list = temp;
    return topEl;
};

/**
 * Get the top element.
 * @return {number}
 */
MyStack.prototype.top = function() {
    return this.topEl
};

/**
 * Returns whether the stack is empty.
 * @return {boolean}
 */
MyStack.prototype.empty = function() {
    return this.list.length === 0
};

作者：egerton
链接：https://leetcode-cn.com/problems/implement-stack-using-queues/solution/gen-ju-guan-fang-ti-jie-xie-de-javascriptshi-xian-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

