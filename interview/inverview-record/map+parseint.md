# Map+ParseInt

## 题目

#### [`['1', '2', '3'].map(parseInt)` what & why ?](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/4)

## 解析1

主要是讲**JS的映射与解析**\
早在 2013年, 加里·伯恩哈德就在微博上发布了以下代码段:

```
['10','10','10','10','10'].map(parseInt);
// [10, NaN, 2, 3, 4]
```

#### parseInt

`parseInt()` 函数解析一个字符串参数，并返回一个指定基数的整数 (数学系统的基础)。

```
const intValue = parseInt(string[, radix]);
```

`string` 要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用 ToString 抽象操作)。字符串开头的空白符将会被忽略。

`radix` 一个介于2和36之间的整数(数学系统的基础)，表示上述字符串的基数。默认为10。\
`返回值` 返回一个整数或NaN

```
parseInt(100); // 100
parseInt(100, 10); // 100
parseInt(100, 2); // 4 -> converts 100 in base 2 to base 10
```

**注意：**\
在`radix`为 undefined，或者`radix`为 0 或者没有指定的情况下，JavaScript 作如下处理：

* 如果字符串 string 以"0x"或者"0X"开头, 则基数是16 (16进制).
* 如果字符串 string 以"0"开头, 基数是8（八进制）或者10（十进制），那么具体是哪个基数由实现环境决定。ECMAScript 5 规定使用10，但是并不是所有的浏览器都遵循这个规定。因此，永远都要明确给出radix参数的值。
* 如果字符串 string 以其它任何值开头，则基数是10 (十进制)。

更多详见[parseInt | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global\_Objects/parseInt)

#### map

`map()` 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

```
var new_array = arr.map(function callback(currentValue[,index[, array]]) {
 // Return element for new_array
 }[, thisArg])
```

可以看到`callback`回调函数需要三个参数, 我们通常只使用第一个参数 (其他两个参数是可选的)。\
`currentValue` 是callback 数组中正在处理的当前元素。\
`index`可选, 是callback 数组中正在处理的当前元素的索引。\
`array`可选, 是callback map 方法被调用的数组。\
另外还有`thisArg`可选, 执行 callback 函数时使用的this 值。

```
const arr = [1, 2, 3];
arr.map((num) => num + 1); // [2, 3, 4]
```

更多详见[Array.prototype.map() | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global\_Objects/Array/map)

#### 回到真实的事例上

回到我们真实的事例上

```
['1', '2', '3'].map(parseInt)
```

对于每个迭代`map`, `parseInt()`传递两个参数: **字符串和基数**。\
所以实际执行的的代码是：

```
['1', '2', '3'].map((item, index) => {
	return parseInt(item, index)
})
```

即返回的值分别为：

```
parseInt('1', 0) // 1
parseInt('2', 1) // NaN
parseInt('3', 2) // NaN, 3 不是二进制
```

所以：

```
['1', '2', '3'].map(parseInt)
// 1, NaN, NaN
```

由此，加里·伯恩哈德例子也就很好解释了，这里不再赘述

```
['10','10','10','10','10'].map(parseInt);
// [10, NaN, 2, 3, 4]
```

#### 如何在现实世界中做到这一点

如果您实际上想要循环访问字符串数组, 该怎么办？ `map()`然后把它换成数字？使用编号!

```
['10','10','10','10','10'].map(Number);
// [10, 10, 10, 10, 10]
```

## 解析2

以为自己真明白了，过去一段时间，再看此题，又模糊了，那，这次真正把它弄明白!\
准备只是，进制：\
2进制，基数只能为 0，1\
3进制，基数为0，1，2\
4进制，基数为0，1，2，3\
5进制，基数为0，1，2，3，4\
...\
8进制，基数为0，1，2，3，4，5，6，7\
10进制，基数为0，1，2，3，4，5，6，7，8，9\
16进制，基数为0，1，2，3，4，5，6，7，8，9，a,b,c,d,e,f\
发现一个规律，基数们都小于 < 进制数\
如 2进制数 的基数 0 和1 都小于2对吧！

继续往下看:\
parseInt(str, radix)\
str：字符串\
radix: 几进制呀\
当 radix >0 && (radix>36 || radix < 2) 返回NAN\
当 radix = 0 或没传即（radix=undefined）返回十进制

```
 当正常情况下，
 str左起第一个数一旦大于进制数radix，立马返回 NaN
 str左起第一个数一旦小于进制数radix，就去做运算，直到遇到一个大于等于radix，就不加了哈！
 如parseInt('4215213', 5)   //   4*5 + 1 = 21
```

```
['1', '2', '3'].map(parseInt) 
让其变形为 
parseInt('1', 0);    // 1
parseInt('2', 1);    // NaN
parseInt('3', 2);   // 由于 2的二进制数是 0 和 1 组成的，所以要返回 NaN
parseInt('1222', 2)   //首位 1<2哎，  则 1
parseInt('213', 3);     // 2* 3 + 1 = 7;  简单的我会算
['10', '10', '10', '10', '10'].map(parseInt)
parseInt('10', 0);    //  10 无意义吧
parseInt('10', 1);    //  NaN   ’10‘ 的首位 1>=1啦！直接NaN
parseInt('10', 2);   //  1*2 + 0 = 2
parseInt('10', 3)   //   1*3 + 0 = 3
parseInt('10', 4);  //   1*4 + 0 = 4  
```
