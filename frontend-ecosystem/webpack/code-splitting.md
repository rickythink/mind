# Code Splitting

## 调整Entry points

如果增加了额外的模块`another-module.js`

```diff
webpack-demo
|- package.json
|- webpack.config.js
|- /dist
|- /src
  |- index.js
+ |- another-module.js
|- /node_modules
```

那么调整`webpack.config.js`

```diff
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
+   another: './src/another-module.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

这会输出如下的编译结果

```
...
            Asset     Size   Chunks             Chunk Names
another.bundle.js  550 KiB  another  [emitted]  another
  index.bundle.js  550 KiB    index  [emitted]  index
Entrypoint index = index.bundle.js
Entrypoint another = another.bundle.js
...
```

这种方法比较便捷，总结来说会有两点不足

1. 如果两个entry有重复的引入，那么会重复打包至两个entry中
2. 不够灵活，不能自动动态分割代码

## SplitChunksPlugin

> CommonsChunkPlugin 在webpack4中已经被移除，现在都统一使用 SplitChunksPlugin

```diff
  const path = require('path');

  module.exports = {
    mode: 'development',
    entry: {
      index: './src/index.js',
      another: './src/another-module.js',
    },
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
+   optimization: {
+     splitChunks: {
+       chunks: 'all',
+     },
+   },
  };
```

