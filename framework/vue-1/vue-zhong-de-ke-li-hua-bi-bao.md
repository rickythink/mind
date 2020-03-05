# vue中的柯里化闭包

makeMap

创建 对象 map，返回函数，用于后面查找 某个东西是否存在 map 中

```javascript
function makeMap( str,  expectsLowerCase ) {    
    var map = Object.create(null);   
    var list = str.split(',');    
    for (var i = 0; i < list.length; i++) {
        map[list[i]] = true;
    }    
    return expectsLowerCase ?        
        function(val) { return map[val.toLowerCase()]; } :        
        function(val) { return map[val]; }
}
```

应用

```javascript
// 应用
var isUnaryTag = makeMap(   
 'area,base,br,col,embed,frame,hr,img,input,isindex,keygen,' +  
 'link,meta,param,source,track,wbr');

// 查找 area 标签是否存在 上面保存过的 字符串中
isUnaryTag('area')
```

