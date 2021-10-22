# Object.create(null)与{}

## Object.create实现

```javascript
Object.create = funtion(o){
  var f = function(){};
  f.prototype = o;
  return new f();
}
```

## Object.create(null)输出

```javascript
let a = Object.create(null)
console.log(a）

------------------
{}
no properties
```

## {}输出

```javascript
let o = {}
console.log(o)

--------------------
{}
__proto__:
constructor
hasOwnProperty
isPrototypreOf
...
```

使用`create`创建的对象，没有任何属性，显示`No properties`，我们可以把它当作一个非常**纯净**的map来使用，我们可以自己定义`hasOwnProperty`、`toString`方法，不管是有意还是不小心，我们完全不必担心会将原型链上的同名方法覆盖掉。

```javascript
//Demo1:
var a= {...省略很多属性和方法...};
//如果想要检查a是否存在一个名为toString的属性，你必须像下面这样进行检查：
if(Object.prototype.hasOwnProperty.call(a,'toString')){
    ...
}
//为什么不能直接用a.hasOwnProperty('toString')?因为你可能给a添加了一个自定义的hasOwnProperty
//你无法使用下面这种方式来进行判断,因为原型上的toString方法是存在的：
if(a.toString){}

//Demo2:
var a=Object.create(null)
//你可以直接使用下面这种方式判断，因为存在的属性，都将定义在a上面，除非手动指定原型：
if(a.toString){}
```
