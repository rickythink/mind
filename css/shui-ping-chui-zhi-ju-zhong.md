# 水平垂直居中

### vertical-align + text-align 方案

```css
#outer {
    width:400px; // 方便看效果
    height:500px; // 方便看效果
    display:table-cell;
    vertical-align:middle;
    text-align:center;
}
```

### flex 方案

```css
#outer{
    display:flex;
    justify-content:center;
    aligns-item:center;
}
```

### 绝对定位

```css
#outer {
    position: relative;
}
#inner {
    top:50%;
    left:50%;
    transform: translate(-50%, -50%);
}
```

