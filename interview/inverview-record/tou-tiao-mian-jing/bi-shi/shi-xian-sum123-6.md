# 实现sum(1)(2)(3) = 6

这是一个柯里化题

```javascript
function add(x,y,z){
	return x+y+z
}


function curry(fn){
  judge = (...args) => args.length === fn.length ? fn(...args) : (arg) => judge(...args, arg)
  return judge
}

const sum = curry(add)

let ret = sum(1)(2)(3)
console.log(ret) // 6
```
