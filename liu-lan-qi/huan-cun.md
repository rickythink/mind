# 缓存

> 1. [https://github.com/ljianshu/Blog/issues/23](https://github.com/ljianshu/Blog/issues/23)
> 2. [https://juejin.im/post/5c136bd16fb9a049d37efc47](https://juejin.im/post/5c136bd16fb9a049d37efc47)

![](../.gitbook/assets/image%20%2836%29.png)

## 缓存位置

### Memory Cache

Memory Cache 也就是内存中的缓存，主要包含的是当前中页面中已经抓取到的资源,例如页面上已经下载的样式、脚本、图片等。内存缓存虽然读取高效，可是缓存持续性很短，会随着进程的释放而释放。 **一旦我们关闭 Tab 页面，内存中的缓存也就被释放了**。

当我们访问过页面以后，再次刷新页面，可以发现很多数据都来自于内存缓存.

内存缓存在缓存资源时并不关心返回资源的HTTP缓存头Cache-Control是什么值，同时资源的匹配也并非仅仅是对URL做匹配，还可能会对Content-Type，CORS等其他特征做校验。

### Dist Cache

**浏览器会把哪些文件丢进内存中？哪些丢进硬盘中？**  
关于这点，网上说法不一，不过以下观点比较靠得住：

* 对于大文件来说，大概率是不存储在内存中的，反之优先
* 当前系统内存使用率高的话，文件优先存储进硬盘

### Service Worker

Service Worker 是运行在浏览器背后的独立线程，一般可以用来实现缓存功能。使用 Service Worker的话，传输协议必须为 HTTPS。因为 Service Worker 中涉及到请求拦截，所以必须使用 HTTPS 协议来保障安全。**Service Worker 的缓存与浏览器其他内建的缓存机制不同，它可以让我们自由控制缓存哪些文件、如何匹配缓存、如何读取缓存，并且缓存是持续性的**。

### Push State

Push Cache（推送缓存）是 HTTP/2 中的内容，当以上三种缓存都没有命中时，它才会被使用。**它只在会话（Session）中存在，一旦会话结束就被释放，并且缓存时间也很短暂**，在Chrome浏览器中只有5分钟左右，同时它也并非严格执行HTTP头中的缓存指令。

## 缓存策略

### 强缓存

#### Expires

缓存过期时间，用来指定资源到期的时间，是服务器端的具体的时间点

```text
Expires: Wed, 22 Oct 2018 08:41:00 GMT
```

Expires 是 HTTP/1 的产物，受限于本地时间，如果修改了本地时间，可能会造成缓存失效。

#### Cache-Control

```text
Cache-Control:max-age=300
```

![](../.gitbook/assets/image%20%2853%29.png)

#### Expires和Cache-Control的区别

其实这两者差别不大，区别就在于 Expires 是http1.0的产物，Cache-Control是http1.1的产物，**两者同时存在的话，Cache-Control优先级高于Expires**；在某些不支持HTTP1.1的环境下，Expires就会发挥用处。所以Expires其实是过时的产物，现阶段它的存在只是一种兼容性的写法。

### 协商缓存

#### 1.Last-Modified和If-Modified-Since

浏览器在第一次访问资源时，服务器返回资源的同时，在response header中添加 Last-Modified的header，值是这个资源在服务器上的最后修改时间，浏览器接收后缓存文件和header；

```text
Last-Modified: Fri, 22 Jul 2016 01:47:00 GMT
```

浏览器下一次请求这个资源，浏览器检测到有 Last-Modified这个header，于是添加If-Modified-Since这个header，值就是Last-Modified中的值；服务器再次收到这个资源请求，会根据 If-Modified-Since 中的值与服务器中这个资源的最后修改时间对比，如果没有变化，返回304和空的响应体，直接从缓存读取，如果If-Modified-Since的时间小于服务器中这个资源的最后修改时间，说明文件有更新，于是返回新的资源文件和200。

**Last-Modified 存在一些弊端：**

* 如果本地打开缓存文件，即使没有对文件进行修改，但还是会造成 Last-Modified 被修改，服务端不能命中缓存导致发送相同的资源
* 因为 Last-Modified 只能以秒计时，如果在不可感知的时间内修改完成文件，那么服务端会认为资源还是命中了，不会返回正确的资源

既然根据文件修改时间来决定是否缓存尚有不足，能否可以直接根据文件内容是否修改来决定缓存策略？所以在 HTTP / 1.1 出现了 `ETag` 和`If-None-Match`

#### 2.ETag和If-None-Match

**Etag是服务器响应请求时，返回当前资源文件的一个唯一标识\(由服务器生成\)，只要资源有变化，Etag就会重新生成**。浏览器在下一次加载资源向服务器发送请求时，会将上一次返回的Etag值放到request header里的If-None-Match里，服务器只需要比较客户端传来的If-None-Match跟自己服务器上该资源的ETag是否一致，就能很好地判断资源相对客户端而言是否被修改过了。如果服务器发现ETag匹配不上，那么直接以常规GET 200回包形式将新的资源（当然也包括了新的ETag）发给客户端；如果ETag是一致的，则直接返回304知会客户端直接使用本地缓存即可。

#### 3.两者之间对比：

* 首先在精确度上，Etag要优于Last-Modified。

Last-Modified的时间单位是秒，如果某个文件在1秒内改变了多次，那么他们的Last-Modified其实并没有体现出来修改，但是Etag每次都会改变确保了精度；如果是负载均衡的服务器，各个服务器生成的Last-Modified也有可能不一致。

* 第二在性能上，Etag要逊于Last-Modified，毕竟Last-Modified只需要记录时间，而Etag需要服务器通过算法来计算出一个hash值。
* 第三在优先级上，服务器校验优先考虑Etag

### 总结

强制缓存优先于协商缓存进行，若强制缓存\(Expires和Cache-Control\)生效则直接使用缓存，若不生效则进行协商缓存\(Last-Modified / If-Modified-Since和Etag / If-None-Match\)，协商缓存由服务器决定是否使用缓存，若协商缓存失效，那么代表该请求的缓存失效，返回200，重新返回资源和缓存标识，再存入浏览器缓存中；生效则返回304，继续使用缓存。

## 应用

#### 1.频繁变动的资源

> Cache-Control: no-cache

对于频繁变动的资源，首先需要使用`Cache-Control: no-cache` 使浏览器每次都请求服务器，然后配合 ETag 或者 Last-Modified 来验证资源是否有效。这样的做法虽然不能节省请求数量，但是能显著减少响应数据大小。

#### 2.不常变化的资源

> Cache-Control: max-age=31536000

通常在处理这类资源时，给它们的 Cache-Control 配置一个很大的 `max-age=31536000` \(一年\)，这样浏览器之后请求相同的 URL 会命中强制缓存。而为了解决更新的问题，就需要在文件名\(或者路径\)中添加 hash， 版本号等动态字符，之后更改动态字符，从而达到更改引用 URL 的目的，让之前的强制缓存失效 \(其实并未立即失效，只是不再使用了而已\)。  
在线提供的类库 \(如 `jquery-3.3.1.min.js`, `lodash.min.js` 等\) 均采用这个模式。

## 用户行为

* 打开网页，地址栏输入地址： 查找 disk cache 中是否有匹配。如有则使用；如没有则发送网络请求。
* 普通刷新 \(F5\)：因为 TAB 并没有关闭，因此 memory cache 是可用的，会被优先使用\(如果匹配的话\)。其次才是 disk cache。
* 强制刷新 \(Ctrl + F5\)：浏览器不使用缓存，因此发送的请求头部均带有 `Cache-control: no-cache`\(为了兼容，还带了 `Pragma: no-cache`\),服务器直接返回 200 和最新内容。

