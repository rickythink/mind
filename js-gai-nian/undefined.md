# !!

有的地方能看到`!!` 这样的操作符，实际上这是`!` 的两次使用。

其真实目的是把一个非`boolean`变量转换为`boolean` 变量, 还有这样写法比较简洁。

```javascript
!!false === false
!!true === true

!!0 === false
!!parseInt("foo") === false // NaN is falsy
!!1 === true
!!-1 === true  // -1 is truthy

!!"" === false // empty string is falsy
!!"foo" === true  // non-empty string is truthy
!!"false" === true  // ...even if it contains a falsy value

!!window.foo === false // undefined is falsy
!!null === false // null is falsy

!!{} === true  // an (empty) object is truthy
!![] === true  // an (empty) array is truthy; PHP programmers beware!
```



