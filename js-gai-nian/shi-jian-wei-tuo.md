# 事件委托

## 基本概念

事件委托，通俗地来讲，就是把一个元素响应事件（click、keydown......）的函数委托到另一个元素。

一般来讲，会把一个或者一组元素的事件委托到它的父层或者更外层元素上，真正绑定事件的是外层元素，当事件响应到需要绑定的元素上时，会通过事件冒泡机制从而触发它的外层元素的绑定事件上，然后在外层元素上去执行函数。

## 事件冒泡与捕获

DOM 中事件委托的实现是利用事件冒泡的机制。

{% hint style="info" %}
addEventListener有三个参数：

> element.addEventListener\(event, function, useCapture\)

第一个参数是需要绑定的事件  
第二个参数是触发事件后要执行的函数  
第三个参数默认值是false，表示在**事件冒泡阶段**调用事件处理函数，如果参数为true，则表示在**事件捕获阶段**调用处理函数。
{% endhint %}

![](../.gitbook/assets/image%20%2877%29.png)

## 事件委托的优点

### 内存小

每一个不必都要绑定函数，统一绑定到父元素

### 动态绑定

新增的子元素也可一并监听到

## 原生实现事件委托

```javascript
function delegate(agent,type,selctor,fn) {
  //agent.addEventListener(type,fn)如果是这样fn中的this会指向agent
  agent.addEventListener(type,function(e){
      let target = e.target         //target指向实际点击的最里层的元素
      let ctarget = e.currentTarget //ctarget会永远指向agent
      let bubble = true

      while(bubble && target != ctarget){
          if(target.matches(selctor)){
            //改变this的指向.
            bubble = fn.call(target,e) === false ? false : true
          }
          target = target.parentNode //模拟事件冒泡
          if(!bubble){
            //  如果fn有return false 那么阻止默认行为
            e.preventDefault()
          }
      }
  },false)
}
```

应用

```javascript
var list = document.getElementById('list')
delegate(list,'click','li',function(e){
  console.log(`${e.target.innerText}   currentTarget : ${e.currentTarget.id}`)
})
delegate(list,'click','#l2',function(e){
  console.log(`${e.target.id}   currentTarget : ${e.currentTarget.id} `)
})
```

关于`target` 和`currentTarget` , **currentTarget始终是监听事件者，而target是事件的真正发出者**。



