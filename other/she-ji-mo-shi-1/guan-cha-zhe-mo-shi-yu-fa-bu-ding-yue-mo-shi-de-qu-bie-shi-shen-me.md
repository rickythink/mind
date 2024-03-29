# 观察者模式与发布-订阅模式的区别是什么？

韩梅梅把所有的开发者拉了一个群，直接把需求文档丢给每一位群成员，这种**发布者直接触及到订阅者**的操作，叫观察者模式。但如果韩梅梅没有拉群，而是把需求文档上传到了公司统一的需求平台上，需求平台感知到文件的变化、自动通知了每一位订阅了该文件的开发者，这种**发布者不直接触及到订阅者、而是由统一的第三方来完成实际的通信的操作，叫做发布-订阅模式**。

相信大家也已经看出来了，观察者模式和发布-订阅模式之间的区别，在于是否存在第三方、发布者能否直接感知订阅者（如图所示）。

![](https://user-gold-cdn.xitu.io/2019/4/5/169ed4923a4f51b7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)观察者模式

![](https://user-gold-cdn.xitu.io/2019/4/5/169ed4a77700d98e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)发布-订阅模式

在我们见过的这些例子里，韩梅梅拉钉钉群的操作，就是典型的观察者模式；而通过EventBus去实现事件监听/发布，则属于发布-订阅模式。

既生瑜，何生亮？既然有了观察者模式，为什么还需要发布-订阅模式呢？

大家思考一下：为什么要有观察者模式？观察者模式，解决的其实是模块间的耦合问题，有它在，即便是两个分离的、毫不相关的模块，也可以实现数据通信。但观察者模式仅仅是减少了耦合，**并没有完全地解决耦合问题**——被观察者必须去维护一套观察者的集合，这些观察者必须实现统一的方法供被观察者调用，两者之间还是有着说不清、道不明的关系。

而发布-订阅模式，则是快刀斩乱麻了——发布者完全不用感知订阅者，不用关心它怎么实现回调方法，事件的注册和触发都发生在独立于双方的第三方平台（事件总线）上。发布-订阅模式下，实现了完全地解耦。
