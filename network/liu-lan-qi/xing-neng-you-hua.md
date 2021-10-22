# 性能优化

## 类型

在前端渲染领域，主要有以下几种方式可供选择：

|    | CSR                                                    | 预渲染                                                           | SSR                                       | 同构                                                                                            |
| -- | ------------------------------------------------------ | ------------------------------------------------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------------- |
| 优点 | <p>不依赖数据</p><p>FP 时间最快</p><p>客户端用户体验好</p><p>内存数据共享</p> | <p> 不依赖数据</p><p>FCP 时间比 CSR 快</p><p>客户端用户体验好</p><p>内存数据共享</p> | <p>SEO 友好</p><p>首屏性能高，FMP 比 CSR 和预渲染快</p> | <p>SEO 友好</p><p>首屏性能高，FMP 比 CSR 和预渲染快</p><p>客户端用户体验好</p><p>内存数据共享</p><p>客户端与服务端代码公用，开发效率高</p> |
| 缺点 | <p>SEO 不友好</p><p>FCP 、FMP 慢</p>                        | <p>SEO 不友好</p><p>FMP 慢</p>                                    | <p>客户端数据共享成本高</p><p>模板维护成本高</p>           | Node 容易形成性能瓶颈                                                                                 |

## 首屏白屏

浏览器渲染包含 HTML 解析、DOM 树构建、CSSOM 构建、JavaScript 解析、布局、绘制等等，大致如下图所示：

![](https://user-gold-cdn.xitu.io/2018/11/16/1671b9d4d735398e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

要搞清楚为什么会有白屏，就需要利用这个理论基础来对实际项目进行具体分析。通过 DevTools 进行分析：

![](https://user-gold-cdn.xitu.io/2018/11/16/1671b9db467e04ce?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

* 等待 HTML 文档返回，此时处于白屏状态。
* 对 HTML 文档解析完成后进行首屏渲染，因为项目中对加了灰色的背景色，因此呈现出灰屏。
* 进行文件加载、JS 解析等过程，导致界面长时间出于灰屏中。
* 当 Vue 实例触发了 mounted 后，界面显示出大体框架。
* 调用 API 获取到时机业务数据后才能展示出最终的页面内容。

## FP/FCP/FMP

#### 首次绘制（First Paint，FP） <a href="shou-ci-hui-zhi-firstpaintfp" id="shou-ci-hui-zhi-firstpaintfp"></a>

我曾经说我们只有两个指标，这其实不太准确。（Chrome）开发者工具还给我们提供了一个指标 - FP。这个指标表示页面绘制的时间点，换句话说它表示当用户第一次看到白屏的时间点.

![](https://cdn-images-1.medium.com/max/800/1\*IuI-OeOiJByd\_kbOnQ4T6A.png)

#### **首次内容绘制（First Contentful Paint，FCP）**

这是当用户看见一些“内容”元素被绘制在页面上的时间点。和白屏是不一样的，它可以是文本的首次出现，或者 SVG 的首次出现，或者 Canvas 的首次出现等等。

因此，用户可能会产生疑问，**它正在运行吗？** 页面是否在他（她）键入 URL 并按 enter 键后开始加载了呢？

![First Paint vs First Contentful Paint of msn.com](https://cdn-images-1.medium.com/max/800/1\*UduDmCWTDefC6CHubA-lTQ.png)

继续看一下 Chromium，FCP 事件在文本（正在等待字体文件加载的文本不计算在内）、图片、Canvas 等元素绘制期间就已经被触发了。因此，FP 和 FCP 的时间差异可能从几毫秒到几秒不等。这个差别甚至可以从上面的图片中看出来。这就是为什么用一个指标来表示真实的首次内容绘制是有价值的。

> 你可以从[这里](https://docs.google.com/document/d/1kKGZO3qlBBVOSZTf-T8BOMETzk3bY15SC-jsMJWv4IE/edit#)阅读所有的规范说明。

**FCP 指标如何对开发者产生价值？**

如果**首次内容绘制**耗时太长，那么：

* 你的网络连接可能有性能问题
* 资源太过庞大（如 index.html），传输它们消耗太多时间

#### 首次有意义绘制（First Meaningful Paint，FMP） <a href="shou-ci-you-yi-yi-hui-zhi-firstmeaningfulpaintfmp" id="shou-ci-you-yi-yi-hui-zhi-firstmeaningfulpaintfmp"></a>

这是指页面主要内容出现在屏幕上的时间点，因此，**它有用吗？**

![First Paint vs First Contentful Paint vs First Meaningful Paint of msn.com](https://cdn-images-1.medium.com/max/800/1\*835Kq5Mzw87L8XRoXXyKIw.png)

主要内容是什么？

当如下显示的时候

* 博客的标题和文本
* 搜索引擎的搜索文本
* 对于电子商务产品来说重要的图片

但如果展示的是

* 下拉菜单或类似的东西
* 无样式内容闪烁（FOUC）
* 导航条或页面标题

则**不计算**在主要内容之内。

> FMP = 最大布局变化时的绘制

基于 Chromium 的实现，这个绘制是使用 [LayoutAnalyzer](https://code.google.com/p/chromium/codesearch#chromium/src/third\_party/WebKit/Source/core/layout/LayoutAnalyzer.h\&sq=package:chromium\&type=cs) 进行计算的，它会收集所有的布局变化，当布局发生最大变化时得出时间。而这个时间就是 FMP。

> 你可以从[这里](https://docs.google.com/document/d/1BR94tJdZLsin5poeet0XoTW60M0SjvOJQttKT-JK8HI/edit#)阅读所有的规范说明。

**FMP 指标如何对开发者产生帮助？**

如果主要内容很久都没有展示出来，那么：

* 太多资源（图片、样式、字体、JavaScript）有较高的加载优先级，因此，它们阻塞了 FMP
