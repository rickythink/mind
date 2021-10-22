# 实现链式调用

## 题目

实现

```javascript
show(1000,()=>{console.log(1)})
    .show(1200,()=>{console.log(2)})
    .show(1300,()=>{console.log(3)})
```

完成等待1s，打印1，之后等待1.2s，打印2，之后打印1.3s，打印3

## 思路

1. 首先这个是链式调用，那么必然是返回一个变量，并且变量拥有show方法
2. 需要把延时任务统一串联起来，注意要用闭包共享状态，启动调用开始时间

## 实现

```javascript
const show = (time, fn) => {
	const task = []
  let isRun = false
  const run = ({time,fn}) =>{
  	isRun = true
  	setTimeout(()=>{
    	fn()
      task.shift()
  		isRun = false
      checkRun()
    }, time)
  }
  const checkRun = ()=> {
    if(task.length && !isRun){
    	run(task[0])
    }
  }
  const wrap = (time, fn) => {
  	task.push({time,fn})
    checkRun()
    return {show:wrap}
  }
  return wrap(time, fn)
}
```

