# 配置记录

## 移除console.log

### TerserPlugin

```javascript
optimization: {
  minimizer: [
    new TerserPlugin({
      sourceMap: true,
      parallel: true,
      terserOptions: {
        compress: {
          warnings: false,
          drop_debugger: true,
          drop_console: true
        }
      }
    }),
  ]
}
```

### UglifyPlugin

```javascript
const UglifyJSPlugin = require('uglifyjs-webpack-plugin')

...

optimization: {
  minimizer: [
    new UglifyJSPlugin({
      uglifyOptions: {
        compress: {
          drop_console: true,
        }
      }
    })
  ]
}
```

### babel-plugin-transform-remove-console

```javascript
module: {
  rules: [
    {
      test: /\.js$/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-react', '@babel/preset-env'],
          plugins: [
            'babel-plugin-transform-remove-console'
          ],
        },
      }
    }
  ]
}
```

