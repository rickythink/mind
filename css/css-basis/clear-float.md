# 清除浮动

### 1.使用空标签 

给所有浮动标签后面添加一个空标签，定义CSS clear:both. 但是这种方法会增加一个无意义的标签。

### 2.设置父级元素为BFC元素 

### 3.使用after伪元素清除浮动 

这个方法只适用于非IE浏览器。该方法必须为需要清除浮动元素的伪对象中设置height:0,不然该元素会比实际元素高出若干像素。

```css
#parent:after{
    content:".";
    height:0;
    visibility:hidden;
    display:block;
    clear:both;
}
```

