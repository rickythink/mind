# CSS画三角形

一个正常的矩形是下面这样的

![](../../.gitbook/assets/image%20%2881%29.png)

当这样设置时，能得到一个梯形

```css
border-color: transparent transparent rgb(0,0,0) transparent;
border-width: 10px 10px 10px 10px
```

![](../../.gitbook/assets/image%20%2870%29.png)

当把div的宽度缩小为0时，三角形就会出来

```javascript
border-color: transparent transparent rgb(0,0,0) transparent;
border-width: 10px 100px 150px 100px;
width: 0px;
```

![](../../.gitbook/assets/image%20%2843%29.png)

不同的border-width控制了三角形的形状，设置如下的css可以生成直角三角形

```css
border-color: transparent transparent rgb(0,0,0) transparent;
border-width: 10px 0px 150px 100px;
width: 0px;
```

![](../../.gitbook/assets/image%20%2876%29.png)

