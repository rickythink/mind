# 盒模型

盒模型是刚学前端的时候就会接触到的问题。

元素都是按照盒模型的规则布局在页面中的。盒模型由 `margin + border + padding + content` 四个属性组成，分为两种：W3C的标准盒模型和IE盒模型。

## **W3C的标准盒模型**

`width = content`，不包含 `border + padding`

![](../.gitbook/assets/image%20%2861%29.png)

## **IE盒模型**

`width = border + padding + content`

![](../.gitbook/assets/image%20%2869%29.png)

## **相互转换**

二者之间可以通过CSS3的 `box-sizing` 属性来转换。

`box-sizing: content-box` 是W3C盒模型

`box-sizing: border-box` 是IE盒模型

## BFC布局规则

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

> [https://segmentfault.com/a/1190000009545742](https://segmentfault.com/a/1190000009545742)

  


