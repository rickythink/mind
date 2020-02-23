# scope css

通过 Webpack 调用 VueJS 中相应 Loader , 给组件HTML模板添加自定义属性 \(Attribute\) `data-v-x`, 以及给组件内CSS选择器添加对应的属性选择器 \(Attribute Selector\) `[data-v-x]`, 达到组件内样式只能生效与组件内HTML的效果, 代码效果如下:

```markup
<div class='lionad' data-v-lionad></div>
<style>
.lionad[data-v-lionad] {
  background: @tiger-orange;
}
</style>
```



