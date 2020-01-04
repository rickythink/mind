# 红绿灯Promise问题

## 题目

题目：红灯三秒亮一次，绿灯一秒亮一次，黄灯2秒亮一次；如何让三个灯不断交替重复亮灯？（用 Promse 实现）

三个亮灯函数已经存在：

```javascript
function red(){
    console.log('red');
}
function green(){
    console.log('green');
}
function yellow(){
    console.log('yellow');
}
```

## 思路

1. 时间差异是由于 setTimeout 不同时间 resolve Promise
2. 不端交替是使用了递归调用（第一次没有做出来）

## 实现

```javascript
function red(){
  console.log('red');
}
function green(){
  console.log('green');
}
function yellow(){
  console.log('yellow');
}

const run = () => 
    Promise.resolve()
      .then(()=>wrap(red, 5)) // 易错
      .then(()=>wrap(green, 2))
      .then(()=>wrap(yellow,1))
      .then(()=>run())


function wrap(fn, time) {
  return new Promise((resolve, reject) =>{
    setTimeout(
    () => {
      fn()
      resolve()
    }, time*1000)
  })
}

run()

// 易错点 注意 wrap不可以被{}包括。没有{}表示有return
// 这样可以等待内部的setTimeout的resolve才会return resolve到下一个then
```

