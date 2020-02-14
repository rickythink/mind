# 三栏布局

三栏布局是很常见的一种页面布局方式。左右固定，中间自适应。实现方式有很多种方法。

**第一种：flex**

```markup
<div class="container">
    <div class="left">left</div>
    <div class="main">main</div>
    <div class="right">right</div>
</div>
.container{
    display: flex;
}
.left{
    flex-basis:200px;
    background: green;
}
.main{
    flex: 1;
    background: red;
}
.right{
    flex-basis:200px;
    background: green;
}
```

**第二种：position + margin**

```markup
<div class="container">
    <div class="left">left</div>
    <div class="right">right</div>
    <div class="main">main</div>
</div>
body,html{
    padding: 0;
    margin: 0;
}
.left,.right{
    position: absolute;
    top: 0;
    background: red;
}
.left{
    left: 0;
    width: 200px;
}
.right{
    right: 0;
    width: 200px;
}
.main{
    margin: 0 200px ;
    background: green;
}
```

**第三种：float + margin**

```markup
<div class="container">
    <div class="left">left</div>
    <div class="right">right</div>
    <div class="main">main</div>
</div>
body,html{
    padding:0;
    margin: 0;
}
.left{
    float:left;
    width:200px;
    background:red;
}
.main{
    margin:0 200px;
    background: green;
}
.right{
    float:right;
    width:200px;
    background:red;
}
```

