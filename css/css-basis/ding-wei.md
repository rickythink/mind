# 定位

## static

静态定位，也即默认定位

```markup
<h1>Basic document flow</h1>

<p>I am a basic block level element. My adjacent block level elements sit on new lines below me.</p>

<p>By default we span 100% of the width of our parent element, and we are as tall as our child content. Our total width and height is our content + padding + border width/height.</p>

<p>We are separated by our margins. Because of margin collapsing, we are separated by the width of one of our margins, not both.</p>

<p>inline elements <span>like this one</span> and <span>this one</span> sit on the same line as one another, and adjacent text nodes, if there is space on the same line. Overflowing inline elements will <span>wrap onto a new line if possible (like this one containing text)</span>, or just go on to a new line if not, much like this image will do: <img src="https://mdn.mozillademos.org/files/13360/long.jpg"></p>
```

![](../../.gitbook/assets/image%20%28113%29.png)

`static`定位所导致的元素位置，是浏览器自主决定的，所以这时`top`、`bottom`、`left`、`right`这四个属性无效。

## relative

与静态定位非常相似，但是可以通过`top` `left` `right` `bottom` 更改最终位置

```text
top: 30px;
left: 30px;
```

![](../../.gitbook/assets/image%20%28110%29.png)

## absolute

绝对定位，按照父元素中最近的relative元素进行定位。

绝对定位使元素的位置与文档流无关，因此不占据空间。这一点与相对定位不同，相对定位实际上被看作普通流定位模型的一部分，因为元素的位置相对于它在普通流中的位置。

## fixed

`fixed`表示，相对于视口（viewport，浏览器窗口）进行偏移，即定位基点是浏览器窗口。这会导致元素的位置不随页面滚动而变化，好像固定在网页上一样。

## sticky

`sticky`跟前面四个属性值都不一样，它会产生动态效果，很像`relative`和`fixed`的结合：一些时候是`relative`定位（定位基点是自身默认位置），另一些时候自动变成`fixed`定位（定位基点是视口）。

`sticky`生效的前提是，必须搭配`top`、`bottom`、`left`、`right`这四个属性一起使用，不能省略，否则等同于`relative`定位，不产生"动态固定"的效果。原因是这四个属性用来定义"偏移距离"，浏览器把它当作`sticky`的生效门槛。

它的具体规则是，当页面滚动，父元素开始脱离视口时（即部分不可见），只要与`sticky`元素的距离达到生效门槛，`relative`定位自动切换为`fixed`定位；等到父元素完全脱离视口时（即完全不可见），`fixed`定位自动切换回`relative`定位。

## float

浮动定位，和绝对定位一样，会脱离文档流。

浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样。

