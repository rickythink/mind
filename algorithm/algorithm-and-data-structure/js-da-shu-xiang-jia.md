# JS 大数相加

当有两个整数 a 和 b ，在通常情况下我们有“+”运算符对其进行相加运算：

```javascript
let sum = a + b;
```

但是 JS 在存放整数的时候是有一个**安全范围**的，一旦数字超过这个范围便会**损失精度。**

我们不能拿精度损失的数字进行运行，因为运算结果一样是会损失精度的。

所以，我们要用**字符串**来表示数据！（不会丢失精度）

> JS 中整数的最大安全范围可以查到是：9007199254740991

假如我们要进行 9007199254740991 + 1234567899999999999

我们要先准备两个字符串变量和一个方法：

```javascript
let a = "9007199254740991";
let b = "1234567899999999999";

function add(a ,b){
   //...
}
```

然后将字符串长度对齐：

```javascript
let a = "9007199254740991";
let b = "1234567899999999999";

function add(a ,b){
   //取两个数字的最大长度
   let maxLength = Math.max(a.length, b.length);
   //用0去补齐长度
   a = a.padStart(maxLength , 0);//"0009007199254740991"
   b = b.padStart(maxLength , 0);//"1234567899999999999"
}
```

然后从个位开始相加：

```javascript
let a = "9007199254740991";
let b = "1234567899999999999";

function add(a ,b){
   //取两个数字的最大长度
   let maxLength = Math.max(a.length, b.length);
   //用0去补齐长度
   a = a.padStart(maxLength , 0);//"0009007199254740991"
   b = b.padStart(maxLength , 0);//"1234567899999999999"
   //定义加法过程中需要用到的变量
   let t = 0;
   let f = 0;   //"进位"
   let sum = "";
   for(let i=maxLength-1 ; i>=0 ; i--){
      t = parseInt(a[i]) + parseInt(b[i]) + f;
      f = Math.floor(t/10);
      sum = t%10 + sum;
   }
   if(f == 1){
      sum = "1" + sum;
   }
   return sum;
}
```

运行：

```javascript
add(a ,b); //结果为：1243575099254740990
```

{% hint style="info" %}
乘法可以基于加法实现，100\*12可以转化成：add( mul('100' , '2') , mul('100' , '1') + '0' )
{% endhint %}
