---
description: 防抖函数总结
---

# Debounce

## 定义

bounce即弹起，debounce的中文意思为“防抖“

## 场景

debounce个人理解的使用场景为，某种经常会触发的事件，等待稳定后最后一次性执行。比较典型的有：

* 键盘输入事件捕捉。等待用户最后输入完成
* scroll事件，等待用户最后滚动结束处理
* resize事件，等待用户最后调整结束处理

## 实现

```javascript
/// 返回一个函数，但是只要它一直持续地被调用，就不会被触发；除非它最后一次被调用停止了n毫秒
// 如果`immediate`参数被传递，那么会直接执行
function debounce(func, wait, immediate){
  var timeout = null;
  
  return excuteFunction(){
    var context = this;
    var args = arguments;
    var later = function(){
      timeout = null
      if(!immediate) func.apply(context, args);
    }
    
    var callNow = immediate && !timeout;
    if(callNow) func.apply(context, args);
    
    // clearTimeout非常关键。如果在timeout完成后later调用前，excuteFunction又被调用，那么timeou会被清除，并接下来被重新赋值开始
    clearTimeout(timeout)
    
    timeout = setTimeout(later, wait);
  }
}
```

## ES6 实现

```javascript
function debounceES6(func, wait, immediate) {
  let timeout
  return (...args) => {
    let callNow = immediate && !timeout
    if (callNow) func.apply(this, args);
    
    clearTimeout(timeout);
    
    timeout = setTimeout(() => {
      timeout = null
      if (!immediate) func.apply(this, args)
    }, wait);
  }
}
```

## 深度

GitHub有用户专门提到利用箭头函数对于this的处理，省略到context的写法

```javascript
function debounce (fn, wait) {
  let t
  return function () {
    clearTimeout(t)
    t = setTimeout(() => fn.apply(this, arguments), wait)
  }
}
```

虽然绝大数情况下箭头函数会调用语义上正确的this，不过考虑如下场景，就会有错误

```javascript
const obj = {
  name: 'foo',
  sayMyName() {
    console.log('My name is', this.name)
  }
}

obj.sayMyName() //-> My name is foo
obj.deb = debounce(obj.sayMyName, 1000)
obj.deb() // Should log -> My name is foo
```

在省略context的写法下，只能输出，因为`My name is`obj的this没有绑定进去。

除非使用

```javascript
obj.deb = debounce(obj.sayMyName.bind(obj), 1000)
obj.deb() // now -> My name is foo 
```

