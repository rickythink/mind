# flex 布局

任何一个容器都可以指定为 Flex 布局

```css
.box{
    display: flex
}    
```

行内元素也可以使用 Flex 布局

```css
.box{
  display: inline-flex;
}
```

设为 Flex 布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效。

Flex 布局是轴线布局，只能指定"项目"针对轴线的位置，可以看作是**一维布局。**Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是**二维布局**。

## 定位

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）

![](../.gitbook/assets/image%20%2822%29.png)

## 属性

容器可以设置 6 个属性

* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content

### flex-direction 

决定主轴项目的排列方向

```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

![](../.gitbook/assets/image%20%2852%29.png)

