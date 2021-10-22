# readystate

一个[`document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 的 **Document.readyState **属性描述了文档的加载状态。

当该属性值发生变化时，会在[`document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 对象上触发`readystatechange`事件。

一个文档的 **readyState** 可以是以下之一：

* loading / 正在加载[`document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 仍在加载
* interactive / 可交互文档已被解析，"**正在加载**"状态结束，但是诸如图像，样式表和框架之类的子资源仍在加载
* complete / 完成文档和所有子资源已完成加载。表示 `load` 状态的事件即将被触发。

## 与 DOMContentLoaded 对比

```
document.readyState = 'loading'
↓
document.readyState变成'interactive'
↓
'DOMContentLoaded'事件执行
↓
document.readyState变成'complete'
↓
'load'事件执行
```
