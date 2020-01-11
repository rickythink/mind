# Flux

Flux把一个应用分成了4个部分：

* View
* Action
* Dispatcher
* Store

![](../../.gitbook/assets/image%20%28126%29.png)

这个过程有几个需要注意的点：

* Dispatcher 的作用是接收**所有**的 Action，然后发给**所有**的 Store。这里的 Action 可能是 View 触发的，也有可能是其他地方触发的，比如测试用例。转发的话也不是转发给某个 Store，而是所有 Store。
* Store 的改变只能通过 Action，不能通过其他方式。也就是说 Store 不应该有公开的 Setter，所有 Setter 都应该是私有的，只能有公开的 Getter。具体 Action 的处理逻辑一般放在 Store 里。

听听描述看看图，可以发现，Flux的最大特点就是数据都是**单向流动**的。  


