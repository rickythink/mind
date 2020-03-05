# 重载

```typescript
// 重载
interface Direction {
    top: number
    right: number
    bottom: number
    left: number
}

function assigned(all: number): Direction
function assigned(topAndBottom: number, leftAndRight: number): Direction
function assigned(top: number, right: number, bottom: number, left: number): Direction

// 代码实现函数不可被调用
function assigned (a: number, b?: number, c?: number, d?: any) {
    if (b === undefined && c === undefined && d === undefined) {
      b = c = d = a
    } else if (c === undefined && d === undefined) {
      c = a
      d = b
    }
    return {
      top: a,
      right: b,
      bottom: c,
      left: d
    }
}
```

传入不同数量的参数，发现只有三个参数的情况下报错了.

