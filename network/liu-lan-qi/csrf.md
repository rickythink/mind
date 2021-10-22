# CSRF

## 定义

CSRF（Cross-site request forgery）跨站请求伪造：攻击者诱导受害者进入第三方网站，在第三方网站中，向被攻击网站发送跨站请求。利用受害者在被攻击网站已经获取的注册凭证，绕过后台的用户验证，达到冒充用户对被攻击的网站执行某项操作的目的。

一个典型的CSRF攻击有着如下的流程：

* [受害者登录a.com](http://xn--a-f38al5vkzdt61bv7l.com)，这样就拥有了登录凭证（Cookie）。
* [攻击者引诱受害者访问了b.com](http://xn--b-nv6ao4io8bp6po6e00mu47cda4311avpa330h.com)。
* [b.com](http://b.com) 向 [a.com](http://a.com) 发送了一个请求：a.com/act=xx。浏览器会默认携带a.com的cookie
* a.com接收到请求后，对请求进行验证，并确认是受害者的凭证，误以为是受害者自己发送的请求。
* a.com以受害者的名义执行了act=xx。
* 攻击完成，攻击者在受害者不知情的情况下，冒充受害者，让a.com执行了自己定义的操作。

> 实例:
>
> 坏蛋 想到使用 CSRF 的攻击方式，他先自己做一个网站，在网站中放入如下代码： src=”[http://bank.example/withdraw?account=bob\&amount=1000000\&for=Mallory](http://bank.example/withdraw?account=bob\&amount=1000000\&for=Mallory) ”，并且通过广告等诱使 受害者 来访问他的网站。当 受害者 访问该网站时，上述 url 就会从 受害者 的浏览器发向银行，而这个请求会附带 受害者 浏览器中的 cookie 一起发向银行服务器。大多数情况下，该请求会失败，因为他要求 受害者 的认证信息。但是，如果 受害者 当时恰巧刚访问他的银行后不久，他的浏览器与银行网站之间的 session 尚未过期，浏览器的 cookie 之中含有 受害者 的认证信息。这时，悲剧发生了，这个 url 请求就会得到响应，钱将从 受害者 的账号转移到 坏蛋 的账号，而 受害者 当时毫不知情。

## 类型

**常见的攻击类型**

* GET类型的CSRF

GET类型的CSRF利用非常简单，只需要一个HTTP请求，一般会这样利用：

```markup
 <img src="http://bank.example/withdraw?amount=10000&for=hacker" > 
```

在受害者访问含有这个img的页面后，浏览器会自动向`http://bank.example/withdraw?account=xiaoming&amount=10000&for=hacker`发出一次HTTP请求。bank.example就会收到包含受害者登录信息的一次跨域请求。

* POST类型的CSRF

这种类型的CSRF利用起来通常使用的是一个自动提交的表单，如：

```markup
 <form action="http://bank.example/withdraw" method=POST>
    <input type="hidden" name="account" value="xiaoming" />
    <input type="hidden" name="amount" value="10000" />
    <input type="hidden" name="for" value="hacker" />
</form>
<script> document.forms[0].submit(); </script> 
```

访问该页面后，表单会自动提交，相当于模拟用户完成了一次POST操作。

POST类型的攻击通常比GET要求更加严格一点，但仍并不复杂。任何个人网站、博客，被黑客上传页面的网站都有可能是发起攻击的来源，后端接口不能将安全寄托在仅允许POST上面。

* 链接类型的CSRF

链接类型的CSRF并不常见，比起其他两种用户打开页面就中招的情况，这种需要用户点击链接才会触发。这种类型通常是在论坛中发布的图片中嵌入恶意链接，或者以广告的形式诱导用户中招，攻击者通常会以比较夸张的词语诱骗用户点击，例如：

```markup
  <a href="http://test.com/csrf/withdraw.php?amount=1000&for=hacker" taget="_blank">
  重磅消息！！
  <a/>
```

由于之前用户登录了信任的网站A，并且保存登录状态，只要用户主动访问上面的这个PHP页面，则表示攻击成功。

## 防护策略

CSRF通常从第三方网站发起，被攻击的网站无法防止攻击发生，只能通过增强自己网站针对CSRF的防护能力来提升安全性。

上文中讲了CSRF的两个特点：

* CSRF（通常）发生在第三方域名。
* CSRF攻击者不能获取到Cookie等信息，只是使用。

CSRF 攻击是黑客借助受害者的 cookie 骗取服务器的信任，但是黑客并不能拿到 cookie，也看不到 cookie 的内容。另外，对于服务器返回的结果，由于浏览器同源策略的限制，黑客也无法进行解析。

#### 验证 HTTP Referer 字段 / 也就是同源检测

方法的显而易见的好处就是简单易行，网站的普通开发人员不需要操心 CSRF 的漏洞，只需要在最后给所有安全敏感的请求统一增加一个拦截器来检查 Referer 的值就可以。特别是对于当前现有的系统，不需要改变当前系统的任何已有代码和逻辑，没有风险，非常便捷。

但是Referer 的值是由浏览器提供的，虽然 HTTP 协议上有明确的要求，但是每个浏览器对于 Referer 的具体实现可能有差别，并不能保证浏览器自身没有安全漏洞。

#### CSRF Token 存于 session 和 html 中 进行对比验证

前面讲到CSRF的另一个特征是，攻击者无法直接窃取到用户的信息（Cookie，Header，网站内容等），仅仅是冒用Cookie中的信息。

而CSRF攻击之所以能够成功，是因为服务器误把攻击者发送的请求当成了用户自己的请求。那么我们可以要求所有的用户请求都携带一个CSRF攻击者无法获取到的Token。服务器通过校验请求是否携带正确的Token，来把正常的请求和攻击的请求区分开，也可以防范CSRF的攻击。

1.将CSRF Token输出到页面中

首先，用户打开页面的时候，服务器需要给这个用户生成一个Token，该Token通过加密算法对数据进行加密，一般Token都包括随机字符串和时间戳的组合，显然在提交时Token不能再放在Cookie中了，否则又会被攻击者冒用。因此，为了安全起见Token最好还是存在服务器的Session中，之后在每次页面加载时，使用JS遍历整个DOM树，对于DOM中所有的a和form标签后加入Token。这样可以解决大部分的请求，但是对于在页面加载之后动态生成的HTML代码，这种方法就没有作用，还需要程序员在编码时手动添加Token。

2.页面提交的请求携带这个Token

对于GET请求，Token将附在请求地址之后，这样URL 就变成 [http://url?csrftoken=tokenvalue。](http://url/?csrftoken=tokenvalue%E3%80%82) 而对于 POST 请求来说，要在 form 的最后加上：

```markup
 <input type=”hidden” name=”csrftoken” value=”tokenvalue”/>
```

这样，就把Token以参数的形式加入请求了。

3.服务器验证Token是否正确

当用户从客户端得到了Token，再次提交给服务器的时候，服务器需要判断Token的有效性，验证过程是先解密Token，对比加密字符串以及时间戳，如果加密字符串一致且时间未过期，那么这个Token就是有效的。

### CSRF 优化

由于使用Session存储，读取和验证CSRF Token会引起比较大的复杂度和性能问题，目前很多网站采用**Encrypted Token Pattern**方式。这种方法的Token是一个计算出来的结果，而非随机生成的字符串。这样在校验时无需再去读取存储的Token，只用再次计算一次即可。

这种Token的值通常是使用UserID、时间戳和随机数，通过加密的方法生成。这样既可以保证分布式服务的Token一致，又能保证Token不容易被破解。

在token解密成功之后，服务器可以访问解析值，Token中包含的UserID和时间戳将会被拿来被验证有效性，将UserID与当前登录的UserID进行比较，并将时间戳与当前时间进行比较。

### CSRF Token 总结

Token是一个比较有效的CSRF防护方法，只要页面没有XSS漏洞泄露Token，那么接口的CSRF攻击就无法成功。

但是此方法的实现比较复杂，需要给每一个页面都写入Token（前端无法使用纯静态页面），每一个Form及Ajax请求都携带这个Token，后端对每一个接口都进行校验，并保证页面Token及请求Token一致。这就使得这个防护策略不能在通用的拦截上统一拦截处理，而需要每一个页面和接口都添加对应的输出和校验。这种方法工作量巨大，且有可能遗漏。

> 验证码和密码其实也可以起到CSRF Token的作用哦，而且更安全。

> 为什么很多银行等网站会要求已经登录的用户在转账时再次输入密码，现在是不是有一定道理了？

\
我知道但是没怎么用的

* 双重cookie
* samesite cookie

### 总结

* CSRF自动防御策略：同源检测（Origin 和 Referer 验证）。
* CSRF主动防御措施：Token验证 或者 双重Cookie验证 以及配合Samesite Cookie。
* 保证页面的幂等性，后端接口不要在GET页面中做用户操作。

{% embed url="https://juejin.im/post/5bc009996fb9a05d0a055192#heading-8" %}



\


