# window的onload事件和domcontentloaded顺序

页面事件的生命周期：

* `DOMContentLoaded` 事件在DOM树构建完毕后被触发，我们可以在这个阶段使用 JS 去访问元素。
  * `async `和 `defer` 的脚本可能还没有执行。
  * 图片及其他资源文件可能还在下载中。
* `load` 事件在页面所有资源(包括图片)被加载完毕后触发，通常我们不会用到这个事件，因为我们不需要等那么久。
* `beforeunload` 在用户即将离开页面时触发，它返回一个字符串，浏览器会向用户展示并询问这个字符串以确定是否离开。
* `unload` 在用户已经离开时触发，我们在这个阶段仅可以做一些没有延迟的操作，由于种种限制，很少被使用。
* `document.readyState` 表征页面的加载状态，可以在 `readystatechange` 中追踪页面的变化状态：
  * `loading` —— 页面正在加载中。
  * `interactive` —— 页面解析完毕，时间上和 `DOMContentLoaded` 同时发生，不过顺序在它之前。
  * `complete` —— 页面上的资源都已加载完毕，时间上和 `window.onload` 同时发生，不过顺序在他之前。
