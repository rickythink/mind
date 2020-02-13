# Referer

HTTP 协议在请求（request）的头信息里面，设计了一个`Referer`字段，给出"引荐网页"的 URL。

{% hint style="info" %}
`Referer`的正确拼写是`Referrer`，但是写入标准的时候，不知为何，没人发现少了一个字母`r`
{% endhint %}

`Referer`字段实际上告诉了服务器，用户在访问当前资源之前的位置。这往往可以用来用户跟踪。

由于涉及隐私，很多时候不适合发送`Referer`字段。

这里举两个例子，都不适合暴露 URL。

1. 一个是功能 URL，即有的 URL 不要登录，可以访问，就能直接完成密码重置、邮件退订等功能。
2. 另一个是内网 URL，不希望外部用户知道内网有这样的地址。`Referer`字段很可能把这些 URL 暴露出去。

此外，还有一种特殊情况，需要定制`Referer`字段。比如社交网站上，用户在对话中提到某个网址。这时，不希望暴露用户所在的原始网址，但是可以暴露社交网站的域名，让对方知道，是我贡献了你的流量。

## 修改referer

对于用户来说，可以改变浏览器本身的[全局设置](http://kb.mozillazine.org/Network.http.sendRefererHeader)，也可以安装[浏览器扩展](https://browsernative.com/http-referer-control-chrome/)。这里就不详细介绍了。

对于开发者来说，`rel="noreferrer"`属性是最简单的一种方法。`<a>`、`<area>`和`<form>`三个标签可以使用这个属性，一旦使用，该元素就不会发送`Referer`字段。

```markup
<a href="..." rel="noreferrer" target="_blank">xxx</a>
```

上面链接点击产生的 HTTP 请求，不会带有`Referer`字段。

注意，`rel="noreferrer"`采用的是正确的拼写。

### 退出页面重定向

还有一种比较老式的[技巧](https://geekthis.net/post/hide-http-referer-headers/#exit-page-redirect)，但是非常有效，可以隐藏掉原始网址，谷歌和 Facebook 都在使用这种方法。

链接的时候，不要直接跳转，而是通过一个重定向网址，就像下面这样。

```markup
<a  href="/exit.php?url=http%3A%2F%2Fexample.com">Example.com</a>
```

