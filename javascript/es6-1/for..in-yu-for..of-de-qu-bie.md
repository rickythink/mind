# for..in 与 for..of的区别

## for in

for-in 循环主要用于遍历对象属性。

使用场景：判断稀疏数组

```javascript
const arr = [,,2]
for(let i = 0; i < arr.length; i++){
    if(i in arr){
        // 不是稀疏项
        console.log(arr[i]) // 2
    }
}
```

for-in 循环，遍历时不仅能读取对象自身上面的成员属性，也能延续原型链遍历出对象的原型属性。

```javascript
function Parent(){
	this.say = 'hello'
}

//声明一个Peson类
function Person(){
    this.name = "kaola";
    this.age = 24;
    this.func1 = function(){
        
    }
}

Person.prototype = new Parent()

//实例化这个类
var bei = new Person();
//使用for-in遍历这个对象
for(keys in bei){
    // name,age,func1,say
    console.log(keys) 
    // true,true,true,false
    console.log(bei.hasOwnProperty(keys))
}
```

{% hint style="info" %}
obj.hasOwnProperty\(keys\)==true 

表示这个属性是对象的成员属性，而不是原型链属性
{% endhint %}

## for of

一个数据结构只要部署了 Symbol.iterator 属性，就被视为具有 iterator 接口，就可以用 for...of 循环遍历它的成员。也就是说，for...of 循环内部调用的是数据结构的 Symbol.iterator 方法。

对于普通对象，for ... in 循环可以遍历键名，for ... of 循环会报错。

