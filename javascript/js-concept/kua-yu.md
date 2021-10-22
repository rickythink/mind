# 跨域

## &#x20;域

![](<../../.gitbook/assets/image (33).png>)

## 跨域

由于JavaScript的同源策略（是浏览器施加的安全限制），浏览器不能执行其他网站的脚本。

所谓同源是指，协议，域名，端口均相同。

![](<../../.gitbook/assets/image (34).png>)

## JSONP

JSONP 全称为：JSON with padding，可用于解决老版本浏览器的跨域数据访问问题。

由于 web 页面上调用 js 文件不受浏览器同源策略的影响，所以通过 script 标签可以进行跨域请求：

1. 首先前端需要先设置好回调函数，并将其作为 url 的参数。
2. 服务端接收到请求后，通过该参数获取到回调函数名，并将数据放在参数中将其返回
3. 收到结果后因为是 script 标签，所以浏览器会当做是脚本进行运行，从而达到跨域获取数据的目的

jsonp 之所以能够跨域的关键在于页面调用 JS 脚本是不受同源策略的影响，相当于向后端发起一条 http 请求，跟后端约定好函数名，后端拿到函数名，动态计算出返回结果并返回给前端执行 JS 脚本，相当于是一种 "动态 JS 脚本"

接下来我们通过一个实例来尝试：

后端逻辑：

```javascript
// jsonp/server.js
const url = require('url');
	
require('http').createServer((req, res) => {
	const data = {
		x: 10
	};
	// 拿到回调函数名
	const callback = url.parse(req.url, true).query.callback;
	console.log(callback);
	res.writeHead(200);
	res.end(`${callback}(${JSON.stringify(data)})`);

}).listen(3000, '127.0.0.1');

console.log('启动服务，监听 127.0.0.1:3000');
```

前端逻辑：

```javascript
// jsonp/index.html
<script>
    function jsonpCallback(data) {
        alert('获得 X 数据:' + data.x);
    }
</script>
<script src="http://127.0.0.1:3000?callback=jsonpCallback"></script>
```

## CORS

CORS 是一个 W3C 标准，全称是"跨域资源共享"（Cross-origin resource sharing）它允许浏览器向跨源服务器，发出 XMLHttpRequest 请求，从而克服了 ajax 只能同源使用的限制。

CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。

对于开发者来说，CORS 通信与同源的 ajax 通信没有差别，代码完全一样。浏览器一旦发现 ajax 请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

因此，实现 CORS 通信的关键是服务器。只要服务器实现了 CORS 接口，就可以跨域通信。

前端逻辑很简单，只要正常发起 ajax 请求即可:

```javascript
// cors/index.html
<script>
	const xhr = new XMLHttpRequest();
	xhr.open('GET', 'http://127.0.0.1:3000', true);
	xhr.onreadystatechange = function() {
		if(xhr.readyState === 4 && xhr.status === 200) {
			alert(xhr.responseText);
		}
	}
	xhr.send(null);
</script>
```

这似乎跟一次正常的异步 ajax 请求没有什么区别，关键是在服务端收到请求后的处理：

```javascript
// cors/server.js
require('http').createServer((req, res) => {

	res.writeHead(200, {
		'Access-Control-Allow-Origin': 'http://localhost:8080',
		'Content-Type': 'text/html;charset=utf-8',
	});
	res.end('这是你要的数据：1111');

}).listen(3000, '127.0.0.1');

console.log('启动服务，监听 127.0.0.1:3000');
```

关键是在于设置相应头中的 **Access-Control-Allow-Origin**，该值要与请求头中 Origin 一致才能生效，否则将跨域失败。

CORS 的优缺点：

1. 使用简单方便，更为安全
2. 支持 POST 请求方式
3. CORS 是一种新型的跨域问题的解决方案，存在兼容问题，仅支持 IE 10 以上

与JSONP对比

JSONP只支持`GET`请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。

## location.hash

在 url 中，`http://www.baidu.com#helloworld` 的 "#helloworld" 就是 location.hash，改变 hash 值不会导致页面刷新，所以可以利用 hash 值来进行数据的传递，当然数据量是有限的。

假设 `localhost:8080` 下有文件 index.html 要和 `localhost:8081` 下的 data.html 传递消息，index.html 首先创建一个隐藏的 iframe，iframe 的 src 指向 `localhost:8081/data.html`，这时的 hash 值就可以做参数传递。

```javascript
// hash/client/index.html 对应 localhost:8080/index.html
<script>
	let ifr = document.createElement('iframe');
	ifr.style.display = 'none';
	ifr.src = "http://localhost:8081/data.html#data";
	document.body.appendChild(ifr);
	
	function checkHash() {
		try {
			let data = location.hash ? location.hash.substring(1) : '';
			console.log('获得到的数据是：', data);
		}catch(e) {

		}
	}
	window.addEventListener('hashchange', function(e) {
		console.log('获得的数据是：', location.hash.substring(1));
	});
</script>
```

data.html 收到消息后通过 parent.location.hash 值来修改 index.html 的 hash 值，从而达到数据传递。

```javascript
// hash/server/data.html 对应 localhost:8081/data.html
<script>
	switch(location.hash) {
		case "#data":
			callback();
			break;
	}
	function callback() {
		const data = "data.html 的数据"
		try {
			parent.location.hash = data;
		}catch(e) {
			// ie, chrome 下的安全机制无法修改 parent.location.hash
			// 所以要利用一个中间的代理 iframe 
			var ifrproxy = document.createElement('iframe');
			ifrproxy.style.display = 'none';
			ifrproxy.src = 'http://localhost:8080/proxy.html#' + data;     // 该文件在 client 域名的域下
			document.body.appendChild(ifrproxy);
		}
	}
</script>
```

由于两个页面不在同一个域下 IE、Chrome 不允许修改 parent.location.hash 的值，所以要借助于 `localhost:8080` 域名下的一个代理 iframe 的 proxy.html 页面

```javascript
// hash/client/proxy.html 对应 localhost:8080/proxy.html
<script>
    parent.parent.location.hash = self.location.hash.substring(1);
</script>
```

当然这种方法存在着诸多的缺点：

1. 数据直接暴露在了 url 中
2. 数据容量和类型都有限等等

## window.name

window.name（一般在 js 代码里出现）的值不是一个普通的全局变量，而是当前窗口的名字，这里要注意的是每个 iframe 都有包裹它的 window，而这个 window 是 top window 的子窗口，而它自然也有 window.name 的属性，window.name 属性的神奇之处在于 name 值在不同的页面（甚至不同域名）加载后依旧存在（如果没修改则值不会变化），并且可以支持非常长的 name 值（2MB）。

举个简单的例子：

你在某个页面的控制台输入：

```javascript
window.name = "Hello World"
window.location = "http://www.baidu.com"
```

页面跳转到了百度首页，但是 window.name 却被保存了下来，还是 Hello World，跨域解决方案似乎可以呼之欲出了：

前端逻辑：

```javascript
// name/client/index.html 对应 localhost:8080/index.html 
<script>
	let data = '';
	const ifr = document.createElement('iframe');
	ifr.src = "http://localhost:8081/data.html";
	ifr.style.display = 'none';
	document.body.appendChild(ifr);
	ifr.onload = function() {
		ifr.onload = function() {
			data = ifr.contentWindow.name;
			console.log('收到数据:', data);
		}
		ifr.src = "http://localhost:8080/proxy.html";
	}
</script>
```

数据页面：

```javascript
// name/server/data.html 对应 localhost:8081/data.html
<script>
	window.name = "data.html 的数据!";
</script>
```

`localhost:8080index.html` 在请求数据端 `localhost:8081/data.html` 时，我们可以在该页面新建一个 iframe，该 iframe 的 src 指向数据端地址(利用 iframe 标签的跨域能力)，数据端文件设置好 window.name 的值。

但是由于 index.html 页面与该页面 iframe 的 src 如果不同源的话，则无法操作 iframe 里的任何东西，所以就取不到 iframe 的 name 值，所以我们需要在 data.html 加载完后重新换个 src 去指向一个同源的 html 文件，或者设置成 'about:blank;' 都行，这时候我只要在 index.html 相同目录下新建一个 proxy.html 的空页面即可。

## postMessage

postMessage 是 HTML5 新增加的一项功能，跨文档消息传输(Cross Document Messaging)，目前：Chrome 2.0+、Internet Explorer 8.0+, Firefox 3.0+, Opera 9.6+, 和 Safari 4.0+ 都支持这项功能，使用起来也特别简单。

前端逻辑：

```javascript
// postMessage/client/index.html 对应 localhost:8080/index.html
<iframe src="http://localhost:8081/data.html" style='display: none;'></iframe>
<script>
	window.onload = function() {
		let targetOrigin = 'http://localhost:8081';
		window.frames[0].postMessage('index.html 的 data!', targetOrigin);
	}
	window.addEventListener('message', function(e) {
		console.log('index.html 接收到的消息:', e.data);
	});
</script>
```

创建一个 iframe，使用 iframe 的一个方法 postMessage 可以想 `http://localhost:8081/data.html` 发送消息，然后监听 message，可以获得其文档发来的消息。

数据端逻辑：

```javascript
// postMessage/server/data.html 对应 localhost:8081/data.html
<script>
	window.addEventListener('message', function(e) {
		if(e.source != window.parent) {
			return;
		}
		let data = e.data;
		console.log('data.html 接收到的消息:', data);
		parent.postMessage('data.html 的 data!', e.origin);
	});
</script>
```

## document.domain

对于主域相同而子域不同的情况下，可以通过设置 document.domain 的办法来解决，具体做法是可以在 `http://www.example.com/index.html` 和 `http://sub.example.com/data.html` 两个文件分别加上 `document.domain = "example.com"` 然后通过 index.html 文件创建一个 iframe，去控制 iframe 的 window，从而进行交互，当然这种方法只能解决主域相同而二级域名不同的情况，如果你异想天开的把 script.example.com 的 domain 设为 qq.com 显然是没用的，那么如何测试呢？

测试的方式稍微复杂点，需要安装 nginx 做域名映射，如果你电脑没有安装 nginx，请先去安装一下: [nginx](http://nginx.org)

前端逻辑：

```
// domain/client/index.html 对应 sub1.example.com/index.html
<script>
	document.domain = 'example.com';
	let ifr = document.createElement('iframe');
	ifr.src = 'http://sub2.example.com/data.html';
	ifr.style.display = 'none';
	document.body.append(ifr);
	ifr.onload = function() {
		let win = ifr.contentWindow;
		alert(win.data);
	}
</script>
```

数据端逻辑：

```
// domain/server/data 对应 sub2.example.com/data.html
<script>
	document.domain = 'example.com';
	window.data = 'data.html 的数据！';
</script>
```

打开操作系统下的 hosts 文件：mac 是位于 /etc/hosts 文件，并添加：

```
127.0.0.1 sub1.example.com
127.0.0.1 sub2.example.com
```

之后打开 nginx 的配置文件：/usr/local/etc/nginx/nginx.conf，并在 http 模块里添加，记得输入 nginx 启动 nginx 服务：

```
/usr/local/etc/nginx/nginx.conf
http {
    // ...
    server {
        listen 80;
        server_name sub1.example.com;
        location / {
            proxy_pass http://127.0.0.1:8080/;
        }
    }
    server {
        listen 80;
        server_name sub2.example.com;
        location / {
            proxy_pass http://127.0.0.1:8081/;
        }
    }
    // ...
}
```

相当于是讲 `sub1.example.com` 和 `sub2.example.com` 这些域名地址指向本地 `127.0.0.1:80`，然后用 nginx 做反向代理分别映射到 8080 和 8081 端口。

这样访问 `sub1(2).example.com` 等于访问 `127.0.0.1:8080(1)`
