# 空值判断

## 空数组判断

通过上面的类型判断是数组类型，然后它的`length = 0`即可

## 空对象判断

### Object.keys\(\) 的长度为 0

```javascript
var obj = {}
Object.keys(obj).length === 0 // true
```

### 对象转字符串为 {}

```javascript
var obj = {}
var a = JSON.stringify(obj)
a === "{}" // true
```

### Object.getOwnPropertyNames\(\) 长度为空

```javascript
var obj = {}
Object.getOwnPropertyNames(obj).length === 0; // true
```



