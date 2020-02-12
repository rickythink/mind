# 用正则实现trim\(\) 清除字符串两端空格

```javascript
function trim(str) {
    return str.replace(/(^\s*)|(\s*$)/g, "")
}
```

