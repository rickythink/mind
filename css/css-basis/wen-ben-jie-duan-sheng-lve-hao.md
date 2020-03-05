# 文本截断省略号

## 单行文本截断

**核心 CSS 语句**

* overflow: hidden；（文字长度超出限定宽度，则隐藏超出的内容）
* white-space: nowrap；（设置文字在一行显示，不能换行）
* text-overflow: ellipsis；（规定当文本溢出时，显示省略符号来代表被修剪的文本）

**优点**

* 无兼容问题
* 响应式截断
* 文本溢出范围才显示省略号，否则不显示省略号
* 省略号位置显示刚好

**短板**

* 只支持单行文本截断

**适用场景**

* 适用于单行文本溢出显示省略号的情况

## 多行文本截断

**核心 CSS 语句**

* -webkit-line-clamp: 2；（用来限制在一个块元素显示的文本的行数, 2 表示最多显示 2 行。 为了实现该效果，它需要组合其他的WebKit属性）
* display: -webkit-box；（和 1 结合使用，将对象作为弹性伸缩盒子模型显示 ）
* -webkit-box-orient: vertical；（和 1 结合使用 ，设置或检索伸缩盒对象的子元素的排列方式 ）
* overflow: hidden；（文本溢出限定的宽度就隐藏内容）
* text-overflow: ellipsis；（多行文本的情况下，用省略号“…”隐藏溢出范围的文本\)

**优点**

* 响应式截断
* 文本溢出范围才显示省略号，否则不显示省略号
* 省略号显示位置刚好

**短板**

* 兼容性一般： -webkit-line-clamp 属性只有 WebKit 内核的浏览器才支持。不过移动端支持比较好

## LESS方案

```css
.wrapline(@line: 1) when(@line <=1) { 
  overflow: hidden; 
  text-overflow: ellipsis; 
  white-space: nowrap; 
} 
 
.wrapline(@line: 2) when(@line > 1) { 
  overflow: hidden; 
  // text-overflow: ellipsis; //不是必须的 
  display: -webkit-box; 
  -webkit-line-clamp: @line; 
  -webkit-box-orient: vertical; 
} 
 
.wrap-line { 
  width: 110px; 
  .wrapline(2); 
}
```

## scss方案

```css
/**
* 溢出省略号
* @param {Number} 行数
*/
@mixin ellipsis($rowCount: 1) {
  @if $rowCount <=1 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  } @else {
    min-width: 0;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: $rowCount;
    -webkit-box-orient: vertical;
  }
}
```



