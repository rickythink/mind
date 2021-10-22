# Let  知识点

## 回顾var

通过 var 声明的变量存在变量提升的特性：

```javascript
if (condition) {
    var value = 1;
}
console.log(value);
```

初学者可能会觉得只有 condition 为 true 的时候，才会创建 value，如果 condition 为 false，结果应该是报错，然而因为变量提升的原因，代码相当于：

```javascript
var value;
if (condition) {
    value = 1;
}
console.log(value);
```

如果 condition 为 false，结果会是 undefined。

var声明的变量会自动挂载在`window` 对象上，而`let` 和`const` 就不会

```javascript
var a = 1
let b = 1
const c = 1
console.log(window.b) // undefined
console.log(window.c) // undefined
```

## 作用域

* let是块级作用域
* var是全局作用域

### 声明

let声明的变量只在代码块中有效

```javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

### For 循环

`var`的声明在for循环中是全局式的

```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```

因此let更适合在for循环中使用

```javascript
for (let i = 0; i < 10; i++) {
  // ...
}

console.log(i);
// ReferenceError: i is not defined
```

因此`let`声明下，最后可以输出正确的值；此外，注意下面的`i`虽然每次赋值，但是JavaScript能记住上次的值

```javascript
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

此外, `for`的括号和方括号可以是两个独立的子作用域

```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

## 变量提升

使用var时，如果变量在声明之前使用，会发生变量提升，导致报错输出undefiend

```javascript
console.log(foo); // 输出undefined
var foo = 2;
```

使用let时，会报错为ReferenceError

```javascript
console.log(bar); // 报错ReferenceError
let bar = 2;
```

变量提升带来的传统错误例子：

```javascript
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
```

进入函数块时，由于内部变量提升，导致tmp值为undefined

## 暂时性锁区(temporal dead zone, TDZ)

块级作用域中存在let命令时，变量就绑定在当前域，不受外部影响

```javascript
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

### 比较隐蔽的死区

```javascript
function bar(x = y, y = 2) {
  return [x, y];
}

bar(); // 报错
```

是 由于x的默认值是另一个参数y，但是y还未赋值。

同样，还有这样的语句

```javascript
// 不报错
var x = x;

// 报错
let x = x;
// ReferenceError: x is not defined
```

也是因为let带来的死区造成的。

总结来说，暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。
