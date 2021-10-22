# is

`is` 可以缩小联合类型的类型推导。例子如下：

```typescript
function isString(test: any): test is string{
    return typeof test === 'string';
}

function example(foo: number | string){
    if(isString(foo)){
        console.log('it is a string' + foo);
        console.log(foo.length); // string function
    }
}
example('hello world');
```

由于使用`is` 关键字，则能推导foo为`string` 类型。那么`foo.length` 就不会报错。

如果把`test is string` 改为`boolean` ，那么`foo.length`会直接报错
