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

## ProgressBarPlugin

把打包的进度以进度条的形式显示出来，同时也可以自定义显示百分比的格式样式。

```javascript
var ProgressBarPlugin = require('progress-bar-webpack-plugin');
// ...
plugins: [  
    new ProgressBarPlugin({
        format: '  build [:bar] ' + chalk.green.bold(':percent') + ' (:elapsed seconds)',
        clear: false    
        })
    ]
```

## SpeedMeasurePlugin

这款插件能将每一个plugin每一个loader的打包时间以及总打包时长统计出来。

## BundleAnalyzerPlugin

在打包结束之后能将各个包的内容、信息、占比以图形化界面表现出来，界面中还有其他的交互和过滤器。



