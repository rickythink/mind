# XSS

## **概念**

XSS（Cross Site Scripting，跨站脚本攻击），为了和层叠样式表（Cascading Style Sheet，CSS）区分，所以安全领域改为叫XSS。\
XSS攻击能让攻击者在受害者的浏览器中执行js脚本，并劫持用户会话、破坏网络或将用户重定向到其他恶意的站点。

## **存储型 **

利用漏洞提交恶意 JavaScript 代码，比如在input, textarea等所有可能输入文本信息的区域，输入`<script src="http://恶意网站"></script>`等，提交后信息会存在服务器中，当用户再次打开网站请求到相应的数据，打开页面，恶意脚本就会将用户的 Cookie 信息等数据上传到黑客服务器。

存储型（持续型）： 攻击过后，效果会一直存在，直到攻击被删除。比如一条腾讯新闻，将攻击代码插入在评论区，那么后面的用户都可能触发这个xss攻击。，可能是一张图片，用户一旦点击就会访问另一个地址，也许不用点击，只要访问就会触发。存储型危害特别大，特别是社交网站。如果一篇日志，里面有xss攻击代码，很多然转载阅读之后都可能会被盗取cookie。

## 反射型

反射型（非持续型）： 一次攻击，将修改好的链接发送给目标用户，让客户点击，达到攻击用户的目的，只能一次性，通过参数提交的方式。

## DOM型

代码示例

```markup
<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>DOM型XSS重现</title>
</head>
<body>
	<script><!--js脚本-->
		function test(){
			var str = document.getElementById("text").value;
			document.getElementById("t").innerHTML = "<a href='"+str+"'>testLink</a>";
		}
	</script>
	<div id="t"></div>
	<input type="text" id="text" ="text" value="" />
	<input type="button" id="s" value="url" onclick="test()" />
</body>
</html>
```

利用payload`'><img src=# onerror="alert('XSS')"><'`执行XSS攻击

## 总结

| XSS类型 | 反射型             | 存储型                                   | DOM型            |
| ----- | --------------- | ------------------------------------- | --------------- |
| 触发过程  | 正常用户访问携带XSS的url | <p>1.黑客构造XSS脚本<br>2.用户访问有XSS脚本的网站</p> | 正常用户访问携带XSS的url |

## XSS预防 <a href="xss-yu-fang" id="xss-yu-fang"></a>

因为XSS的根本就是向网站插入脚本代码，并使它运行的一种手段。防御方法分为两种，服务端防御和客户端防御。

**cookie** **secure属性**\
当设置为true时，表示创建的 Cookie 会被以安全的形式向服务器传输（ssl），即只能在 HTTPS 连接中被浏览器传递到服务器端进行会话验证，如果是 HTTP 连接则不会传递该信息，所以不会被窃取到Cookie 的具体内容。

**cookie** **HttpOnly属性**\
如果在Cookie中设置了"HttpOnly"属性，那么通过程序(JS脚本、Applet等)将无法读取到Cookie信息

服务器设置

1. **设置cookir HttpOnly,secure**\
   可以限制javascript不能读取cookie，防止会话ID泄露
2. **处理富文本**\
   过滤掉富文本中的敏感标签如（script、iframe、form），还有敏感词（javascript:） 等等

客户端防御：

1. **输入检查**\
   防止输入敏感字段，如javascript、cookie等等
2. **检查输出**\
   脚本都是通过混淆在HTML当中，被当成html代码的一部分才得到执行。

\
