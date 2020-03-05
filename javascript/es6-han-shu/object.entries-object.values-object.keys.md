# Object.entries/Object.values/Object.keys

Object.keys 遍历 Object 的key

```javascript
const family = {
  father: "Jonathan Kent",
  mother: "Martha Kent",
  son: "Clark Kent",
}

Object.keys(family);
// ["father", "mother", "son"]
```

Object.values 遍历 Object 的值

```javascript
Object.values(family);
// ["Jonathan Kent", "Martha Kent", "Clark Kent"]
```

Object.entries 返回一个 \[ key, values \] 形式的数组。 数组的顺序和 Object 如何定义的没有联系。

```javascript
const object1 = {
  a: 'somestring',
  b: 42
};

for (let [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}

// expected output:
// "a: somestring"
// "b: 42"
// order is not guaranteed
```

对于 Object.entries 输入为非对象，会自动转换为对象再操作

```javascript
// non-object argument will be coerced to an object
console.log(Object.entries('foo')); // [ ['0', 'f'], ['1', 'o'], ['2', 'o'] ]

// returns an empty array for any primitive type, since primitives have no own properties
console.log(Object.entries(100)); // [ ]
```

