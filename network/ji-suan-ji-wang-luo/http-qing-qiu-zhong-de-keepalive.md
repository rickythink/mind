# HTTP请求中的keep-alive

在http早期，每个http请求都要求打开一个tpc socket连接，并且使用一次之后就断开这个tcp连接。 使用keep-alive可以改善这种状态，即在**一次TCP连接中可以持续发送多份数据而不会断开连接**。

通过使用keep-alive机制，可以减少tcp连接建立次数，也意味着可以减少TIME\_WAIT状态连接，以此提高性能和提高httpd服务器的吞吐率\(更少的tcp连接意味着更少的系统内核调用,socket的accept\(\)和close\(\)调用\)。 

但是，keep-alive并不是免费的午餐,长时间的tcp连接容易导致系统资源无效占用。配置不当的keep-alive，有时比重复利用连接带来的损失还更大。所以，正确地设置keep-alive timeout时间非常重要。



