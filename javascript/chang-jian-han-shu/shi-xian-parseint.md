# 实现parseInt

当参数 radix 的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。

举例，如果 string 以 "0x" 开头，parseInt() 会把 string 的其余部分解析为十六进制的整数。如果 string 以 0 开头，那么 ECMAScript v3 允许 parseInt() 的一个实现把其后的字符解析为八进制或十六进制的数字。如果 string 以 1 \~ 9 的数字开头，parseInt() 将把它解析为十进制的整数。

只有字符串中的第一个数字会被返回。

留意以下测试用例

```javascript
/ 以下例子均返回15:
console.log(_parseInt("F", 16));
console.log(_parseInt("17", 8));
console.log(_parseInt("15", 10));
console.log(_parseInt(15.99, 10));
console.log(_parseInt("FXX123", 16));
console.log(_parseInt("1111", 2));
console.log(_parseInt("15*3", 10));
console.log(_parseInt("12", 13));

// 以下例子均返回 NaN:
console.log(_parseInt("Hello", 8)); // Not a number at all
console.log(_parseInt("546", 2));   // Digits are not valid for binary representations

// 以下例子均返回 -15：
console.log(_parseInt("-F", 16));
console.log(_parseInt("-0F", 16));
console.log(_parseInt("-0XF", 16));
console.log(_parseInt(-15.1, 10));
console.log(_parseInt(" -17", 8));
console.log(_parseInt(" -15", 10));
console.log(_parseInt("-1111", 2));
console.log(_parseInt("-15e1", 10));
console.log(_parseInt("-12", 13));
// 下例中也全部返回 17，因为输入的 string 参数以 "0x" 开头时作为十六进制数字解释，而第二个参数假如经过 Number 函数转换后为 0 或 NaN，则将会忽略。
console.log(_parseInt("0x11", 16));
console.log(_parseInt("0x11", 0));
console.log(_parseInt("0x11"));

// 下面的例子返回 224
console.log(_parseInt("0e0",16));
```

## 完整实现

```javascript
function _parseInt(string, radix) {
  if (typeof string !== "string" && typeof string !== "number") return NaN;
  if (
    radix &&
    (typeof radix !== "number" ||
    /^[1-9]\d*\.\d*|0\.\d*[1-9]\d*$/.test(radix) || // 识别小数
      radix > 36 ||
      radix < 2)
  )
    return NaN;
  string = String(string);
  var rexp =
      radix === 10 ? /(-?)([0]?)([0-9]+)/ : /(-?)([0]?[Xx]?)([0-9a-fA-F]+)/,
    a = string.match(rexp),
    sign = a[1],
    rawRadix = a[2],
    rawNum = a[3],
    result = 0,
    strArr = rawNum.split(""), // 分割每个字符
    len = strArr.length,
    numArr = [];
  if (a && !radix) {
    if (rawRadix.toUpperCase() === "0X") {
      radix = 16;
    } else if (rawRadix === "0") {
      radix = 8;
    } else {
      radix = 10;
    }
  }
  for (var i = 0; i < len; i++) {
    var num;
    var charCode = strArr[i].toUpperCase().charCodeAt(0);
    if (radix <= 36 && radix >= 11) {
      if (charCode >= 65 && charCode <= 90) {
        // a - z => 10 - 35
        num = charCode - 55;
      } else {
        // 只会是数字了 -48 会转换为数字 0-9 => 48-57
        num = charCode - 48;
      }
    } else {
      num = charCode - 48;
    }
    if (num < radix) {
      numArr.push(num);
    } else {
      return NaN;
    }
  }
  if (numArr.length > 0) {
    numArr.forEach(function(item, j) {
      result += item * Math.pow(radix, numArr.length - j - 1);
    });
  }
  if (sign === "-") {
    result = -result;
  }
  return result;
}

```

## 简单实现

```javascript
function _parseInt(str, radix) {
    let str_type = typeof str;
    let res = 0;
    if (str_type !== 'string' && str_type !== 'number') {
        // 如果类型不是 string 或 number 类型返回NaN
        return NaN
    }

    // 字符串处理
    str = String(str).trim().split('.')[0]
    let length = str.length;
    if (!length) {
        // 如果为空则返回 NaN
        return NaN
    }

    if (!radix) {
        // 如果 radix 为0 null undefined
        // 则转化为 10
        radix = 10;
    }
    if (typeof radix !== 'number' || radix < 2 || radix > 36) {
        return NaN
    }

    for (let i = 0; i < length; i++) {
        let arr = str.split('').reverse().join('');
        res += Math.floor(arr[i]) * Math.pow(radix, i)
    }

    return res;
}
```

剔除其他判断的最核心实现

```javascript
function _parseInt(str, radix) {
    const intStr = String(str).trim().split('.')[0]
    // 翻转字符
    const intStrR = intStr.split('').reverse().join('');
    for (let i = 0; i < intStr.length; i++) {
        res += Math.floor(intStrRr[i]) * Math.pow(radix, i)
    }

    return res;
}
```
