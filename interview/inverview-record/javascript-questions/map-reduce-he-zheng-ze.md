# map, reduce和正则

实现一个get函数，使得下面的调用可以输出正确的结果

```javascript
const obj = { 
    selector: { 
        to: { toutiao: "FE Coder"} 
    }, 
    target: [1, 2, { name: 'byted'}]
};

get(obj, 'selector.to.toutiao', 'target[0]', 'target[2].name');
// [ 'FE Coder', 1, 'byted']
```

思路：

1. 多个入参，内部用arguments或者...args 收集
2. 'selector.to.toutiao' 可以联想到迭代获取

### 有缺陷的第一版

```javascript
function get(data, ...args) {
	return args.map((item) => {
		const paths = item.split('.');
		let res = data;
		paths.map(path => res = res[path]);
		return res;
	})
}
```

缺陷在于没有考虑`target[0]` 的情况。

题目关键在于如何从字符串"target\[2].name"，切分成数组\["target", "2", "name"]。

切分？那可是正则的强项，使用正则/\\\[|\\]|\\./即可，即以 **\[ ] .** 这三个字符为分隔符，比如：

```javascript
"target[2].name".split(/\[|\]|\./)
// => ["target", "2", "", "name"]
```

注意结果里面有空字符。当然，可以再用filter过滤下。

但我们实现时，其实是可以跳过空字符的：

```javascript
["target", "2", "", "name"].reduce((p, c) => {
  if (typeof p === 'undefined' || c === "") return p
  return p[c]
}, obj)
// => 'byted'
```

注意，上面的代码很健壮的，也考虑到"targetxxx\[3].name1"这种不存在的情形。

### 改进后的解法

```javascript
function get(obj, ...args) {
  return args.map(item => {
    return item.split(/\.|\[|\]/)
      .reduce((p, c) => {
        if (typeof p === 'undefined' || c === "") return p
        return p[c]
      }, obj)
  })
}
const obj = { selector: { to: { toutiao: "FE Coder"} }, target: [1, 2, { name: 'byted'}]};
console.log(get(obj, 'selector.to.toutiao', 'target[0]', 'target[2].name'));
// => [ 'FE Coder', 1, 'byted']
```
