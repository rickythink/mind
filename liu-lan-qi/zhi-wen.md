# 指纹

## 定义

“浏览器指纹”是一种通过浏览器对网站可见的配置和设置信息来跟踪Web浏览器的方法

浏览器指纹追踪技术到目前已经进入2.5代。

* 第一代是状态化的，主要集中在用户的cookie和evercookie上，需要用户登录才可以得到有效的信息。
* 第二代才有了浏览器指纹的概念，通过不断增加浏览器的特征值从而让用户更具有区分度，例如（UA、浏览器插件信息）
* 第三代是已经将目光放在人身上了，通过收集用户的行为、习惯来为用户建立特征值甚至模型，可以实现真正的追踪技术，这部分目前实现比较复杂，依然在探索中。

目前处于2.5代是因为现在需要解决的问题是如何解决跨浏览器识别指纹的问题上。

## 信息组成

浏览器指纹也可以进行简单的分为普通指纹和高级指纹，普通指纹可以理解为容易被发现并且容易修改的部分，例如http的header

```javascript
{
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3", 
    "Accept-Encoding": "gzip, deflate, br", 
    "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8", 
    "Host": "httpbin.org", 
    "Sec-Fetch-Mode": "navigate", 
    "Sec-Fetch-Site": "none", 
    "Sec-Fetch-User": "?1", 
    "Upgrade-Insecure-Requests": "1", 
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36"
  }
}
```

**User-Agent**包含了浏览器和操作系统的信息，例如我当前使用的是MacOS，并且使用77版本的chrome。假如在头部刻意伪造了UA，网页也可以通过**navigator.userAgent**来拿到真实的UA。

除开从http中拿到的指纹，也可以通过其他方式拿到浏览器的特性信息，在[这篇文档](https://panopticlick.eff.org/about)中就陈列了一些可行的特征值

* 每个浏览器的用户代理字符串
* 浏览器发送的HTTP ACCEPT标头
* 屏幕分辨率和色彩深度
* 系统设置为时区
* 浏览器中安装的浏览器扩展/插件，例如Quicktime，Flash，Java或Acrobat，以及这些插件的版本
* 计算机上安装的字体，由Flash或Java报告。
* 浏览器是否执行JavaScript脚本
* 浏览器是否能种下各种cookie和“超级cookie（super cookies）”
* 通过Canvas指纹生成的图像的哈希
* WebGL指纹生成的图像的哈希
* 是否浏览器设置为“Do Not Track”
* 系统平台（例如Win32，Linux x86）
* 系统语言（例如，cn,en-US）
* 浏览器是否支持触摸屏

### Cavans指纹

Cavans是HTML5中的动态绘图标签，也可以用它生成图片或者处理图片。即便使用Cavans绘制相同的元素，但是由于系统的差别，字体渲染引擎不同，对抗锯齿、次像素渲染等算法也不同，canvas将同样的文字转成图片，得到的结果也是不同的。

具体代码实现为：

```javascript
function getCanvasFingerprint () {
    var canvas = document.getElementById("anchor-uuid");
    var context = canvas.getContext("2d");
    context.font = "18pt Arial";
    context.textBaseline = "top";
    context.fillText("Hello, user.", 2, 2);
    return canvas.toDataURL("image/jpeg");
}
```

![](../.gitbook/assets/image%20%288%29.png)

在画布上渲染一些文字，再用**toDataURL**转换出来，即便开启了隐私模式一样可以相同的值  


**AudioContex指纹**

AudioContext指纹和Cavans类似也是基于硬件设备或者软件的差别，来产生不同的音频输出，然后计算得到不同的hash来作为标志，当然这里的音频并没有直接在浏览器中播放出来，只需要拿到播放前的处理数据就行，音频指纹测试地址：[audiofingerprint.openwpm.com/](https://audiofingerprint.openwpm.com/)

**WebRTC**

WebRTC（网页实时通信，Web Real Time Communication），是可以让浏览器有音视频实时通信的能力，它提供了三个主要的API来让JS可以实时获取和交换音视频数据，MediaStream、RTCPeerConnection和RTCDataChannel。当然如果要使用WebRTC获得通信能力，用户的真实ip就得暴露出来（NAT穿透），所以RTCPeerConnection就提供了这样的API，直接使用JS就可以拿到用户的IP地址。

#### 跨浏览器指纹

综上提到的浏览器指纹都是从同一个浏览器上获得。但是很多特征值都是不稳定的，例如UA、cavans指纹在相同设备的不同浏览器打开会完全不一样。 同一套浏览器指纹算法在不同浏览器（本小结所说的不同浏览器是指同一台设备上的不同浏览器）上也就不可用了。

![](../.gitbook/assets/image%20%2875%29.png)



