# Jest 内部原理剖析



![](<../.gitbook/assets/image (204).png>)

Jest 是 Facebook 开源的一个 JavaScript 测试框架，已经应用在  [Babel](https://babeljs.io) / [TypeScript](https://www.typescriptlang.org) / [Node](https://nodejs.org/en/) /  [React](https://reactjs.org) /  [Angular](https://angular.io) /  [Vue](https://vuejs.org) 等项目中。

在愉悦地编写 Jest 测试代码后，你是否会好奇 `test` / ` expect` / `mock` / `spyOn` 这些内置方法是如何实现的呢？下面我们一起来看一下。

## Test

Jest 中的 `test` 函数标准实例是：

```javascript
test('Passing test', () => {})
// ✓ Passing test
```

```javascript
test('Failing test', () => new Error('Error message'))
// ✕ Failing test
// Error message
```

这里可以注意到，如果第二个参数的函数执行没有抛出异常的话，会打印第一个参数，因此我们的模拟实现可以是：

```javascript
function test(title, callback) {
  try {
    callback()
    console.log(`✓ ${title}`)
  } catch (error) {
    console.error(`✕ ${title}`)
    console.error(error)
  }
}
```

## Expect

`expect` 函数的典型使用实例是：

```javascript
// 需要测试的函数
function multiply(a, b) {
   return a * b
}

// 测试用例
test('Multipling 3 by 4 is 12', () => {
   expect(multiply(3, 4)).toBe(12)
} // ✓ Multipling 3 by 4 is 12

test('Multipling 3 by 4 is 12', () => {
   expect(multiply(3, 4)).toBe(13)
}) // ✕ Multipling 3 by 4 is 12
   // Expected: 13
   // Received: 12
```

注意到`expect` 的返回值具备一个可以调用的方法`toBe` ，那么我们可以这样模拟`expect`&#x20;

```javascript
function expect(current) {
  return {
    toBe(expected) {
      if (current !== expected) {
        throw new Error(`
          Expected: ${expected}
          Received: ${current}
        `)
      }
    }
  }
}
```

## Mock

当我们要避免测试一个功能时又依赖于不稳定的另外一个功能，我们创建一个 Mock 来更好的管理。Jest 使用 `jest.fn` 是实现 Mock

```javascript
// random.js
function getRandom(min, max) {
    return Math.floor(Math.random() * (max - min) + min)
}
export { getRandom }

// cards.js
import { getRandom } from './random.js'
const getRandomCard = (cards) => {
   const randomCardIndex = getRandom(0, array.length)
   return cards[randomCardIndex]
}
export { getRandomCard }

// cards.test.js
import * as randomGenerator from './random.js'
import { getRandomCard } from './cards.js'
test('Returns 7♥', () => {
    const originalImplementation = randomGenerator.getRandom
    randomGenerator.getRandom = jest.fn(() => 2)
    const result = getRandomCard(['2♣', 'K♦️', '7♥', '3♠'])
    expect(result).toBe('7♥')
    expect(randomGenerator.getRandom).toHaveBeenCalledTimes(1)
    expect(randomGenerator.getRandom).toHaveBeenCalledWith(0, 4)
    // we keep the test idempotent
    randomGenerator.getRandom = originalImplementation
})
```

为了实现上面的效果，我们可以这样模拟定义`jest.fn` 和 `expect`&#x20;

```javascript
// jest.fn
function fn(impl) {
  const mockFn = (...args) => {
    mockFn.mock.calls.push(args)
    return impl(...args)
  }
  mockFn.mock = {calls: []}
  return mockFn
}

// expect
import assert from 'assert'
function expect(current) {
  return {
    toHaveBeenCalledTimes(nrTimesExpected) {
      if (current.mock.calls.length !== nrTimesExpected) {
        throw new Error(`
          Expected: ${expected}
          Called: ${func.mock.calls.length}
        `)
      }
    },
    toHaveBeenCalledWith(...params) {
        // this is a simplified version         
        if (!assert.deepStrictEqual(current.mock.calls[0], ...params)) {
            throw new Error(`
               Expected: ${expected}
               Called: ${func.mock.calls.length}
            `)
        }
    }
  }
}
```
