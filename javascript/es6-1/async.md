# async

## await

`async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。当函数执行的时候，一旦遇到`await`就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

```javascript
async function getStockPriceByName(name) {
  const symbol = await getStockSymbol(name);
  const stockPrice = await getStockPrice(symbol);
  return stockPrice;
}

getStockPriceByName('goog').then(function (result) {
  console.log(result);
});
```

```javascript
async function f() {
  return 'hello world';
}

f().then(v => console.log(v))
// "hello world"
```

## await

`await`命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。

```javascript
async function f() {
  // 等同于
  // return 123;
  return await 123;
}

f().then(v => console.log(v))
// 123
```

`await`命令只能用在`async`函数之中，如果用在普通函数，就会报错。

```javascript
async function dbFuc(db) {
  let docs = [{}, {}, {}];

  // 报错
  docs.forEach(function (doc) {
    await db.post(doc);
  });
}
```

正确的写法是：

```javascript
async function dbFuc(db) {
  let docs = [{}, {}, {}];

  for (let doc of docs) {
    await db.post(doc);
  }
}

// 或者下面的写法
async function dbFuc(db) {
  let docs = [{}, {}, {}]
  let promises = docs.map((doc) => db.post(doc))
  
  let results = []
  for (let promise of promises) {
    results.push(await promise)
  }
  
  console.log(results)
}

// 或者下面的写法
async function dbFun(db) {
  let docs = [{}, {}, {}]
  let promises = docs.map((doc) => db.post(doc))
  
  let results = await Promise.all(promises)
  console.log(results)
}
```

## 案例：按顺序完成异步操作

同时发出远程请求

```javascript
async function logInOrder(urls) {
    // 并发读取远程URL
    const textPromises = urls.map(async url => {
      const response = await fetch(url)
      return respone.text()  
    })
    
    // 按次序输出
    for (const textPromise of textPromises){
      console.log(await textPromise)
    }
}
```

上面代码使用`fetch`方法，同时远程读取一组 URL。每个`fetch`操作都返回一个 Promise 对象，放入`textPromises`数组。然后，`reduce`方法依次处理每个 Promise 对象，然后使用`then`，将所有 Promise 对象连起来，因此就可以依次输出结果。

## 遍历器异步

待补充



