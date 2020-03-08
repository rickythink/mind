# 利用 Cloudflare Worker 定制 Gitbook

![](../.gitbook/assets/image%20%2846%29.png)

支撑本站背后的是 Gitbook v2 版本，曾经开源产品的闭源商业化分支。Gitbook 暂时不提供定制 CSS 的特性。因此我们对于左栏一定会出现的 " Powered by Gitbook "无法做出隐藏的修改。

难道只能等官方提供这个功能后才能实现吗？

非也，这里我来分享一下利用 Cloudflare Worker 来完成这个神奇的功能。

## Cloudflare Worker

Cloudflare Workers的名称来自Web Workers，更具体地说是Service Workers，一个用于在web浏览器后台运行并拦截HTTP请求的脚本的W3C标准API。Cloudflare Workers是针对相同的标准API编写的，但是是在Cloudflare的服务器上运行，而不是在浏览器中运行。

以下是可以使用的工具：

* 使用最新的标准语言功能执行任意JavaScript代码。
* 拦截和修改HTTP请求和响应URL，状态，标头和正文内容。
* 直接从您的Worker响应请求，或将其转发到其他地方。
* 将HTTP请求发送到第三方服务器。
* 以串行或并行方式发送多个请求，并使用响应组成对原始请求的最终响应。
* 在响应已经返回到客户端之后发送异步请求（例如，用于记录或分析）。
* 控制其他Cloudflare功能，例如缓存行为。

我们可以在 [Worker Template](https://developers.cloudflare.com/workers/templates/) 看到各种各样的参考示例。

## mind-ricky-css-inject

我的目标很简单，就是把” Powered by Gitbook " 的 HTML 块隐藏即可。这里所涉及到的步骤如下：

1. 导入域名至 Cloudflare
2. 配置域名的DNS。根域名指向 `hosting.gitbook.com`
3. 创建新的 worker
4. 配置 worker routes。把域名规则和对应worker关联起来
5. 生效

其中最核心的是，创建什么样的 worker 可以完成我们的需求。这个 worker 需要在返回的 HTML 中添加一段 CSS。

```javascript
addEventListener('fetch', event => {
	event.passThroughOnException()
	event.respondWith(handleRequest(event.request))
})

/**
 * Fetch and log a given request object
 * @param {Request} request
 */
async function handleRequest(request) {
	const response = await fetch(request)
	var html = await response.text()

	// Inject scripts
	const customScripts = `<style type="text/css">
    a[role='presentation'] {
      display: none;
    }
  </style></body>`
	html = html.replace( /<\/body>/ , customScripts)

	// return modified response
	return new Response(html, {
		headers: response.headers
	}) 
}
```

如此一来，我们再通过 cloudflare 代理 gitbook 的链接访问，并配置 worker 加载，那么会在 html 中的 body 前注入

```markup
<style type="text/css">
  a[role='presentation'] {
    display: none;
  }
</style>
```

从而实现设置隐藏 gitbook power by 的目的。

