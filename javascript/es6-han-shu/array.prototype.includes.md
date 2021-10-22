# Array.prototype.includes

### `Array.prototype.includes()`

`includes `返回`true` 如果数组中存在某个元素

```javascript
let array = [1,2,4,5];

array.includes(2);
// true
array.includes(3);
// false
```

## 额外用法

`includes` 第二参数可以加入起始查找的索引（涵盖这个起始值）。默认情况下，是0。

```javascript
let array = [1,3,5,7,9,11];

array.includes(3,1);
// find the number 3 starting from array index 1
// true
array.includes(5,4);
//false
array.includes(1,-1);
// find the number 1 starting from the ending of the array going backwards
// false
array.includes(11,-3);
// true
```

