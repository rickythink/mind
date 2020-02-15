# 盒模型

盒模型是刚学前端的时候就会接触到的问题。

元素都是按照盒模型的规则布局在页面中的。盒模型由 `margin + border + padding + content` 四个属性组成，分为两种：W3C的标准盒模型和IE盒模型。

## **W3C的标准盒模型**

`width = content`，不包含 `border + padding`

![](../.gitbook/assets/image%20%2884%29.png)

## **IE盒模型**

`width = border + padding + content`

![](../.gitbook/assets/image%20%2894%29.png)

## **相互转换**

二者之间可以通过CSS3的 `box-sizing` 属性来转换。

`box-sizing: content-box` 是W3C盒模型

`box-sizing: border-box` 是IE盒模型

## IFC\(inline formatting context\)布局规则

`Inline level`的box会参与形成IFC，比如`display`值为`inline，inline-table，inline-block`的元素。

在行内格式化上下文中，框\(boxes\)一个接一个地水平排列，起点是包含块的顶部。水平方向上的 `margin`，`border` 和 `padding`在框之间得到保留。框在垂直方向上可以以不同的方式对齐：它们的顶部或底部对齐，或根据其中文字的基线对齐。包含那些框的长方形区域，会形成一行，叫做行框。

## BFC\(block formatting context\)布局规则

`Block level`的box会参与形成BFC，比如`display`值为`block，list-item，table`的元素。

1. 内部的Box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由`margin`决定。属于同一个BFC的两个相邻Box的`margin`会发生重叠
3. 每个元素的左外边缘（`margin-left`\)， 与包含块的左边（`contain box left`）相接触\(对于从左往右的格式化，否则相反\)。即使存在浮动也是如此。除非这个元素自己形成了一个新的BFC。
4. BFC的区域不会与`float box`重叠。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算BFC的高度时，浮动元素也参与计算

## 如何创建BFC

1. 根元素或其它包含它的元素
2. 浮动 \(元素的 `float` 不是 `none`\)
3. 绝对定位的元素 \(元素具有 `position` 为 `absolute` 或 `fixed`\)
4. 非块级元素具有 `display: inline-block，table-cell, table-caption, flex, inline-flex`
5. 块级元素具有`overflow` ，且值不是 `visible`

## 关于inline-block

inline-block的元素的内部是一个BFC，但是它本身可以和其它inline元素一起形成IFC

## 关于浮动与BFC

![](../.gitbook/assets/image%20%28140%29.png)

> [https://segmentfault.com/a/1190000009545742](https://segmentfault.com/a/1190000009545742)
>
> [https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block\_formatting\_context](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

  


