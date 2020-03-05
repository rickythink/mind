# MutationObserver

## 定义

MutationObserver提供监视DOM树修改的能力。可监听的变化有：属性、文本。支持监听孙子节点。

## 简单示例

```javascript
// 注册监视器, 一旦发生变化会alert
let observer = new MutationObserver(()=>{
    alert('change')
});
// 开始监视，observer可以接受{ attributes: true, childList: true, subtree: true }
observer.observe(el, { childList: true, subtree: true });
// 停止监视, 释放资源
observer.disconnect()
```

## 常用场景

滚动加载, 用来监视元素是否已经加入或者修改子元素, 加入成功后触发回调

```text
// DOM注册scroll事件监听
container.addEventListener('scroll', onScroll);
// 创建MutationObserver对象，并绑定到onScroll事件处理  
const observer = el[scope].observer = new MutationObserver(onScroll);
// 设置监听
observer.observe(container, { childList: true, subtree: true });
```

