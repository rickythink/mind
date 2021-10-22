# 数组去重

```javascript
function removeDup(arr){
  const ret = []
  for(let i = 0; i < arr.length; i ++){
    if(ret.indexOf(arr[i]) < 0){
      ret.push(arr[i])
    }
  }
  return ret
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
