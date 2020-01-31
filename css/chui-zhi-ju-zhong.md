# 垂直局中

**单行文本**

1\) 若元素是单行文本, 则可设置 line-height 等于父元素高度

**行内块级元素**

2\) 若元素是行内块级元素, 基本思想是使用display: inline-block, vertical-align: middle和一个伪元素让内容块处于容器中央.

```css
.parent::after, .son{
    display:inline-block;
    vertical-align:middle;
}
.parent::after{
    content:'';
    height:100%;
}
```

这是一种很流行的方法, 也适应IE7.

**元素高度不定**

3\) 可用 **vertical-align** 属性, 而vertical-align只有在父层为 td 或者 th 时, 才会生效, 对于其他块级元素, 例如 div、p 等, 默认情况是不支持的. 为了使用vertical-align, 我们需要设置父元素display:table, 子元素 display:table-cell;vertical-align:middle;

**优点**

元素高度可以动态改变, 不需再CSS中定义, 如果父元素没有足够空间时, 该元素内容也不会被截断.

**缺点**

IE6~7, 甚至IE8 beta中无效.

4\) 可用 **Flex 2012版**, 这是CSS布局未来的趋势. Flexbox是CSS3新增属性, 设计初衷是为了解决像垂直居中这样的常见布局问题. 相关的文章如《[弹性盒模型Flex指南](https://link.juejin.im?target=http%3A%2F%2Flouiszhai.github.io%2F2017%2F01%2F13%2Fflex%2F)》

父元素做如下设置即可保证子元素垂直居中:

```css
.parent {
  display: flex;
  align-items: center;
}
```

**优点**

* 内容块的宽高任意, 优雅的溢出.
* 可用于更复杂高级的布局技术中.

**缺点**

* IE8/IE9不支持
* 需要浏览器厂商前缀
* 渲染上可能会有一些问题

5\) 使用flex 2009版.

```css
.parent {
      display: box;
      box-orient: vertical;
      box-pack: center;
}
```

**优点**

实现简单, 扩展性强

**缺点**

兼容性差, 不支持IE

6\) 可用 **transform** , 设置父元素相对定位\(position:relative\), 子元素如下css样式:

```css
.son{
    position:absolute;
    top:50%;
    -webkit-transform: translate(-50%,-50%);  
    -ms-transform: translate(-50%,-50%);
    transform: translate(-50%,-50%);
}
```

**优点**

代码量少

**缺点**

IE8不支持, 属性需要追加浏览器厂商前缀, 可能干扰其他 transform 效果, 某些情形下会出现文本或元素边界渲染模糊的现象.

**元素高度固定**

7\) 设置父元素相对定位\(position:relative\), 子元素如下css样式:

```css
.son{
    position:absolute;
    top:50%;
    height:固定;
    margin-top:-0.5高度;
}
```

**优点**

适用于所有浏览器.

**缺点**

父元素空间不够时, 子元素可能不可见\(当浏览器窗口缩小时,滚动条不出现时\).如果子元素设置了overflow:auto, 则高度不够时, 会出现滚动条.

8\) 设置父元素相对定位\(position:relative\), 子元素如下css样式:

```css
.son{
    position:absolute;
    height:固定;
    top:0;
    bottom:0;
    margin:auto 0;
}
```

**优点**

简单

**缺点**

没有足够空间时, 子元素会被截断, 但不会有滚动条.  


