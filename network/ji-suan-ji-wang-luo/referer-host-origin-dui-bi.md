# referer, host, origin对比

## host

Host 请求头指明了请求服务器的域名/IP地址和端口号。

组成：域名+端口号

例子：test.com:1998

如果没有给定端口号，会自动使用被请求服务的默认端口（比如请求一个HTTP的URL会自动使用80端口）。

**HTTP/1.1 的所有请求报文中必须包含一个 Host 头字段**。如果一个 HTTP/1.1 请求缺少 Host 头字段或者设置了超过一个的 Host 头字段，一个400（Bad Request）状态码会被返回。

## referer

在以下几种情况下，Referer 不会被发送：

* 来源页面采用的协议为表示本地文件的 "file" 或者 "data" URI；
* 当前请求页面采用的是非安全协议，而来源页面采用的是安全协议（HTTPS）；
* 直接输入网址或通过浏览器书签访问；
* 使用 JavaScript 的 Location.href 或者是 Location.replace()；
* 使用 html5 中 noreferrer

## origin

请求首部字段 Origin 指示了请求来自于哪个站点。该字段仅指示服务器名称，并不包含任何路径信息。除了不包含路径信息，该字段与 Referer 首部字段相似。

**该首部用于 CORS 请求或者 POST 请求**。



\
