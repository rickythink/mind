# 模拟实现async

async是基于generator的

真实使用

```javascript
async function fn(args) { ... }

// 可以以下来模拟
const asyncFn = wrap(fn)
asyncFn(args) 
```

简版实现

```javascript
const wrap = (genFn) =>{
	return (...args) => new Promise((resolve, reject) => {
  	const g = genFn(...args)
    function step(data){
    	const res = g.next(data)
      if(!res.done){
      	Promise.resolve(res.value).then(step)
      }else{
      	resolve(res.value)
      }
    }
    step()
  })
}
```

完整实现

```javascript
const wrap = (genFn) =>{
	return (...args) => new Promise((resolve, reject) => {
  	const g = genFn(...args)
    function stepF(nextFn){
    	try{
      	const res = nextFn()
      } catch (e){
      	return reject(e)
      }
      if(res.done){
      	return resolve(res.value)
      }
      
      Promise.resolve(res.value).then( v => {
      	stepF( () => g.next(v))
      }, e => {
      	stepF( () => g.throw(e))
      })
    }
    stepF( () => g.next())
  })
}
```

