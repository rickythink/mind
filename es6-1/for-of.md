# For-of

## 数组

### for...in循环读取键名

```javascript
var arr = ['a', 'b', 'c', 'd'];

for (let a in arr) {
    console.log(a); // 0 1 2 3
}
```

### for...of循环读取键值

```javascript
var arr = ['a', 'b', 'c', 'd'];

for (let a in arr) {
    console.log(a); // a b c d
}
```

### entries返回遍历\[键名，键值\]组成的数组

```javascript
let arr = ['a', 'b', 'c'];
for (let pair of arr.entries()) {
    console.log(pair);
}

// [0, 'a']
// [1, 'b']
// [2, 'c']
```

## 对象

普通的对象，for...in循环可以遍历键名，for...of循环会报错。

```javascript
let es6 = {
  edition: 6,
  committee: "TC39",
  standard: "ECMA-262"
};

for (let e in es6) {
  console.log(e);
}
// edition
// committee
// standard

for (let e of es6) {
  console.log(e);
}
// TypeError: es6[Symbol.iterator] is not a function
```

### 解决方案1：

使用 Object.keys 将对象的键名生成一个数组，然后遍历这个数组

```javascript
for (var key of Obejct.keys(someObject)) {
    console.log(key + ': ' + someObejct[key])
}
```

解决方案2：

```javascript
function* entries(obj) {
    for (let key of Object.keys(obj)) {
        yield [key, obj[key]]
    }
}

for (let [key, value] of entries(obj)) {
    consoloe.log(key, '->', value)
}
```

