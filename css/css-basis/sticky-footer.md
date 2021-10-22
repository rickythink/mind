# Sticky Footer

### 全局增加一个负值下边距等于底部高度

```markup
<body>
  <div class="wrapper">

      content

    <div class="push"></div>
  </div>
  <footer class="footer"></footer>
</body>
```

```css
html, body {
  height: 100%;
  margin: 0;
}
.wrapper {
  min-height: 100%;

  /* Equal to height of footer */
  /* But also accounting for potential margin-bottom of last child */
  margin-bottom: -50px;
}
.footer,
.push {
  height: 50px;
}
```

## flex

```markup
<body>
  <div class="content">
    content
  </div>
  <footer class="footer"></footer>
</body>
```

```css
html {
  height: 100%;
}
body {
  min-height: 100%;
  display: flex;
  flex-direction: column;
}
.content {
  flex: 1;
}
```
