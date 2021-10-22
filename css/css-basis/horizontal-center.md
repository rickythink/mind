# 水平居中

1\) 若是行内元素, 给其父元素设置 text-align:center,即可实现行内元素水平居中.

2\) 若是块级元素, 该元素设置 margin:0 auto即可.

3\) 若子元素包含 float:left 属性, 为了让子元素水平居中, 则可让父元素宽度设置为fit-content,并且配合margin, 作如下设置:

```css
.parent{
    width: -moz-fit-content;
    width: -webkit-fit-content;
    width:fit-content;
    margin:0 auto;
}
```

fit-content是CSS3中给width属性新加的一个属性值,它配合margin可以轻松实现水平居中, 目前只支持Chrome 和 Firefox浏览器.

4\) 使用flex 2012年版本布局, 可以轻松的实现水平居中, 父元素设置如下:

```css
.parent{
    display: flex;
    justify-content: center;
}
```

5\) 使用CSS3中新增的transform属性, 子元素设置如下:

```css
.son{
    position:absolute;
    left:50%;
    transform:translate(-50%,0);
}
```

6\) 使用绝对定位方式, 以及负值的margin-left, 子元素设置如下:

```css
.son{
    position:absolute;
    width:固定;
    left:50%;
    margin-left:-0.5宽度;
}
```

7\) 使用绝对定位方式, 以及left:0;right:0;margin:0 auto; 子元素设置如下:

```css
.son{
    position:absolute;
    width:固定;
    left:0;
    right:0;
    margin:0 auto;
}
```
