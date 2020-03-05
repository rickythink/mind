# 变量提升和声明作用域

```javascript
b = c;

b();
console.log(a);    //1
console.log(b);    //2
console.log(c);    //3

function c() {
    a = 1, b = 2, c = 3;
};

// 将上述代码稍作修改:

b = function c() {
    a = 1, b = 2, c = 3;
};

b();
console.log(a);    //1
console.log(b);    //2
console.log(c);    //Uncaught ReferenceError: c is not defined

// 再次将上述代码稍作修改:

b = function c() {
    a = 1, b = 2, c = 3;
    console.log(a);    //1
    console.log(b);    //2
    console.log(c);    //fuction c(){...
};
b();

```

### 解析

函数声明 的优先级 **高于** 变量声明的优先级，但 **不会** 覆盖变量赋值。

> 对于 `var bar = function foo(){};` 语句，其实就是一个有效的命名函数表达式，但有一点需要记住：这个名字只在新定义的函数作用域内有效，因为规范规定了标示符不能在外围的作用域内有效：
>
> ```javascript
> var f = function foo(){
>     return typeof foo; // foo是在内部作用域内有效
> };
> // foo在外部用于是不可见的
> typeof foo; // "undefined"
> f(); // "function"
> ```

记住一点： 命名函数表达式的标示符（即函数名称）在外部作用域是无效的

