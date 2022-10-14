# 基础知识

## 交换函数

### 位运算符交换

```javascript
const swap = (a, b) => {
    a = a ^ b
    b = a ^ b
    a = a ^ b
} 

```

### 解构交换

```javascript
const swap = (a, b) => {
    [a, b] = [b, a]
}
```
