# Cloudflare Worker

Cloudflare Workers的名称来自Web Workers，更具体地说是Service Workers，一个用于在web浏览器后台运行并拦截HTTP请求的脚本的W3C标准API。Cloudflare Workers是针对相同的标准API编写的，但是是在Cloudflare的服务器上运行，而不是在浏览器中运行。

以下是可以使用的工具：

* 使用最新的标准语言功能执行任意JavaScript代码。
* 拦截和修改HTTP请求和响应URL，状态，标头和正文内容。
* 直接从您的Worker响应请求，或将其转发到其他地方。
* 将HTTP请求发送到第三方服务器。
* 以串行或并行方式发送多个请求，并使用响应组成对原始请求的最终响应。
* 在响应已经返回到客户端之后发送异步请求（例如，用于记录或分析）。
* 控制其他Cloudflare功能，例如缓存行为。

## 使用 Cloudfalre worker 隐藏 gitbook 的 power by

Gitbook 目前并不支持定制 CSS。因此我们可以考虑借助 Cloudflare Worker 的力量注入我们自己的 CSS，从而达到覆盖原样式的目的。

这里在 [Worker](https://workers.cloudflare.com) 页面创建新的 Worker，名称记为 `mind-ricky-css-inject` 。添加的 script 如下：

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
