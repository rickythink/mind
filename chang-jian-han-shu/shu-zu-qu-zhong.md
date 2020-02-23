# 数组去重

```javascript
function removeDup(arr){
    var result = [];
    var hashMap = {};
    for(var i = 0; i < arr.length; i++){
        var temp = arr[i]
        if(!(temp in hashMap)){
            hashMap[temp] = true
            result.push(temp)
        }
    }
    return result;
}
```

```javascript
function Uniq(arr = []) {
    return arr.reduce((t, v) => t.includes(v) ? t : [...t, v], []);
}
```

```javascript
Array.from(new Set(arr))
```

```javascript
[...new Set(arr)]
```

