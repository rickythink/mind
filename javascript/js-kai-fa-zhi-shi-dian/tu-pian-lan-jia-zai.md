# 图片懒加载

## clientHeight、scrollTop 和 offsetTop

首先给图片一个占位资源:

```markup
<img src="default.jpg" data-src="http://www.xxx.com/target.jpg" />
```

接着，通过监听 scroll 事件来判断图片是否到达视口:

```javascript
let img = document.getElementsByTagName("img");
let num = img.length;
let count = 0;//计数器，从第一张图片开始计

lazyload();//首次加载别忘了显示图片

window.addEventListener('scroll', lazyload);

function lazyload() {
  let viewHeight = document.documentElement.clientHeight;//视口高度
  let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;//滚动条卷去的高度
  for(let i = count; i <num; i++) {
    // 元素现在已经出现在视口中
    if(img[i].offsetTop < scrollHeight + viewHeight) {
      if(img[i].getAttribute("src") !== "default.jpg") continue;
      img[i].src = img[i].getAttribute("data-src");
      count ++;
    }
  }
}
```

当然，最好对 scroll 事件做节流处理，以免频繁触发:

```javascript
// throttle函数我们上节已经实现
window.addEventListener('scroll', throttle(lazyload, 200));
```

## getBoundingClientRect

现在我们用另外一种方式来判断图片是否出现在了当前视口, 即 DOM 元素的 `getBoundingClientRect` API。

上述的 lazyload 函数改成下面这样:

```javascript
function lazyload() {
  for(let i = count; i <num; i++) {
    // 元素现在已经出现在视口中
    if(img[i].getBoundingClientRect().top < document.documentElement.clientHeight) {
      if(img[i].getAttribute("src") !== "default.jpg") continue;
      img[i].src = img[i].getAttribute("data-src");
      count ++;
    }
  }
}
```

## IntersectionObserver

浏览器内置的一个`API`，实现了`监听window的scroll事件`、`判断是否在视口中`以及`节流`三大功能。

我们来具体试一把：

```javascript
let img = document.getElementsByTagName("img");

const observer = new IntersectionObserver(changes => {
  //changes 是被观察的元素集合
  for(let i = 0, len = changes.length; i < len; i++) {
    let change = changes[i];
    // 通过这个属性判断是否在视口中
    if(change.isIntersecting) {
      const imgElement = change.target;
      imgElement.src = imgElement.getAttribute("data-src");
      observer.unobserve(imgElement);
    }
  }
})
Array.from(img).forEach(item => observer.observe(item));
```

这样就很方便地实现了图片懒加载，当然这个`IntersectionObserver`也可以用作其他资源的预加载，功能非常强大。

![](<../../.gitbook/assets/image (189).png>)
