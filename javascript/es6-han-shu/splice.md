# splice

## 定义

**`splice()`** 方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。**此方法会改变原数组**。

## 语法

```text
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```

### start

指定修改的开始位置（从0计数）。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从-1计数，这意味着-n是倒数第n个元素并且等价于`array.length-n`）；如果负数的绝对值大于数组的长度，则表示开始位置为第0位。

### deleteCount 可选

* 整数，表示要移除的数组元素的个数。如果 `deleteCount` 大于 `start` 之后的元素的总数，则从 `start` 后面的元素都将被删除（含第 `start` 位）。
* 如果 `deleteCount` 被省略了，或者它的值大于等于`array.length - start`\(也就是说，如果它大于或者等于`start`之后的所有元素的数量\)，那么`start`之后数组的所有元素都会被删除。
* 如果 `deleteCount` 是 0 或者负数，则不移除元素。这种情况下，至少应添加一个新元素。

### item1, item2 .. 可选

要添加进数组的元素,从`start` 位置开始。如果不指定，则 `splice()` 将只删除数组元素。

## 例子

```javascript
var myFish = ["angel", "clown", "mandarin", "sturgeon"];
var removed = myFish.splice(2, 0, "drum");

// myFish = removed = ["angel", "clown", "drum", "mandarin", "sturgeon"]
// 删除0个元素，添加了一个元素
```

```javascript
var myFish = ['angel', 'clown', 'drum', 'mandarin', 'sturgeon'];
var removed = myFish.splice(3, 1);

// 运算后的 myFish: ["angel", "clown", "drum", "sturgeon"]
// 被删除的元素: ["mandarin"]
// 删除了一个元素
```

