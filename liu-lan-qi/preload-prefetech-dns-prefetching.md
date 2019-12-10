# Preload, Prefetech,DNS Prefetching

## Preload

[`<link rel="preload">`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Preloading_content) 是一种 resource hint，用来指定页面加载后很快会被用到的资源，所以在页面加载的过程中，我们希望在浏览器开始主体渲染之前尽早 preload。

## Prefetch

[`<link rel="prefetch">`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Link_prefetching_FAQ) 是一种 resource hint，用来告诉浏览器在页面加载完成后，利用空闲时间提前获取用户未来可能会访问的内容。

## DNS Prefetching

DNS prefetching 允许浏览器在用户浏览页面时在后台运行 DNS 的解析。如此一来，DNS 的解析在用户点击一个链接时已经完成，所以可以减少延迟。

```text
<link rel=“dns-prefetch” href="//www.google-analytics.com">
```

