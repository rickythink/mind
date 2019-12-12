# calc

calc函数是css3新增的功能，可以使用calc\(\)计算border、margin、pading、font-size和width等属性设置动态值

```css
#div1 {
    position: absolute;
    left: 50px;
    width: calc( 100% / (100px * 2) );
    //兼容写法
    width: -moz-calc( 100% / (100px * 2) );
    width: -webkit-calc( 100% / (100px * 2) );
    border: 1px solid black;
}
```

* 需要注意的是，运算符前后都需要保留一个空格，例如：width: calc\(100% - 10px\);
* calc\(\)函数支持 "+", "-", "\*", "/" 运算;
* 对于不支持 calc\(\) 的浏览器，整个属性值表达式将被忽略。不过我们可以对那些不支持 calc\(\)的浏览器，使用一个固定值作为回退。

