# 浏览器的本地存储

## cookie

cookie是纯文本，没有可执行代码。存储数据，当用户访问了某个网站（网页）的时候，我们就可以通过cookie来向访问者电脑上存储数据，或者某些网站为了辨别用户身份、进行session跟踪而储存在用户本地终端上的数据（通常经过加密）

1. 一个域名下存放的cookie的个数是有限制的，不同的浏览器存放的个数不一样,一般为20个。
2. 每个cookie存放的内容大小也是有限制的，不同的浏览器存放大小不一样，一般为4KB。
3. cookie也可以设置过期的时间，默认是会话结束的时候，当时间到期自动销毁

### 安全性

通常 cookie 信息都是使用HTTP连接传递数据，这种传递方式很容易被查看，所以 cookie 存储的信息容易被窃取。假如 cookie 中所传递的内容比较重要，那么就要求使用加密的数据传输。

secure选项用来设置cookie只在确保安全的请求中才会发送。当请求是HTTPS或者其他安全协议时，包含 secure 选项的 cookie 才能被发送至服务器。

`document.cookie = "username=cfangxu; secure"`

把cookie设置为secure，只保证 cookie 与服务器之间的数据传输过程加密，而保存在本地的 cookie文件并不加密。就算设置了secure 属性也并不代表他人不能看到你机器本地保存的 cookie 信息。机密且敏感的信息绝不应该在 cookie 中存储或传输，因为 cookie 的整个机制原本都是不安全的

## localstorage

HTML5新方法，不过**IE8及以上**浏览器都兼容。

#### 特点 <a href="item-3-13" id="item-3-13"></a>

* 生命周期：持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。
* 存储的信息在同一域中是共享的。
* 当本页操作（新增、修改、删除）了localStorage的时候，本页面不会触发storage事件,但是别的页面会触发storage事件。
* 大小：据说是5M（跟浏览器厂商有关系）
* 在非IE下的浏览中可以本地打开。IE浏览器要在服务器中打开。
* localStorage本质上是对字符串的读取，如果存储内容多的话会消耗内存空间，会导致页面变卡
* localStorage受同源策略的限制

## sessionStorage

其实跟localStorage差不多，也是本地存储，会话本地存储

#### 特点： <a href="item-4-18" id="item-4-18"></a>

* 用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。也就是说只要这个浏览器窗口没有关闭，即使刷新页面或进入同源另一页面，数据仍然存在。关闭窗口后，sessionStorage即被销毁，或者在新窗口打开同源的另一个页面，sessionStorage也是没有的。

### 上诉三者的区别

### 相同

在本地（浏览器端）存储数据

### 不同：

1. localStorage、sessionStorage、localStorage只要在相同的协议、相同的主机名、相同的端口下，就能读取/修改到同一份localStorage数据。
2. sessionStorage比localStorage更严苛一点，除了协议、主机名、端口外，还要求在同一窗口（也就是浏览器的标签页）下。
3. localStorage是永久存储，除非手动删除。
4. sessionStorage当会话结束（当前页面关闭的时候，自动销毁）
5. cookie的数据会在每一次发送http请求的时候，同时发送给服务器而localStorage、sessionStorage不会。

## web SQL database

使用很少，知道就行

## indexedDB

来自MDN的解释： indexedDB 是一种低级API，用于客户端存储大量结构化数据(包括, 文件/ blobs)。该API使用索引来实现对该数据的高性能搜索。

`indexedDB` 是一个基于JavaScript的面向对象的数据库。 `IndexedDB`允许你存储和检索用键索引的对象;

IndexedDB 鼓励使用的基本模式如下所示：

* 打开数据库并且开始一个事务。
* 创建一个 object store。
* 构建一个请求来执行一些数据库操作，像增加或提取数据等。
* 通过监听正确类型的 DOM 事件以等待操作完成。
* 在操作结果上进行一些操作（可以在 request 对象中找到）

## Application Cache

Application Cache 是一种允许浏览器通过manifest 配置文件在本地有选择性地存储JavaScript、CSS、图片等静态资源的文件级缓存机制。当页面不是首次打开时，通过一个特定的manifest文件配置描述来选择读取本地Application Cache里面的文件。

Application Cache 已经开始慢慢被弃用，被 Service Worker 取代。

## cacheStorage

cacheStorage是在ServiceWorker规范中定义的，可用于保存每个ServiceWorker声明的Cache对象，是未来可能用来代替Application Cache的离线方案。



