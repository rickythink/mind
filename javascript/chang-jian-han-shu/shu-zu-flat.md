# 数组flat

数组flat方法是ES6新增的一个特性，可以将多维数组展平为低维数组。如果不传参默认展平一层，传参可以规定展平的层级。

```javascript
// 展平一级
function flat(arr){
    var result = [];
    for(var i = 0; i < arr.length; i++){
        if(Array.isArray(arr[i])){
            result = result.concat(flat(arr[i]))
        }else{
            result.push(arr[i]);
        }
    }
    return result;
}
```

```javascript
//展平深度层
 function flattenByDeep(array,deep){
      var result = [];
      for(var i = 0 ; i < array.length; i++){
          if(Array.isArray(array[i]) && deep > 1){
                result = result.concat(flattenByDeep(array[i],deep -1))
          }else{
                result.push(array[i])
          }
      }
      return result;
  }
```
