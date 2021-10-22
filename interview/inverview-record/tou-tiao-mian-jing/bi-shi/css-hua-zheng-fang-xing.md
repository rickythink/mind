# CSS 画正方形

## vw 单位

```css
.placeholder {
    width: 50vw;
    height: 50vw;
}
```

## padding-bottom

```css
.placeholder{
    width: 50%;
    padding-bottom: 50%;/* padding百分比相对父元素宽度计算 */
    height: 0; //避免被内容撑开多余的高度
}
```
