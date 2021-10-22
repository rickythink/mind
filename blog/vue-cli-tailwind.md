# vue-cli 项目添加 tailwind.css

最近在尝试工具类思想设计的 CSS 库 tailwind.css，简单记录一下怎么在 vue-cli 工程中引入 tailwind.css。

## 1. 安装 tailwind

```javascript
# Using npm
npm install tailwindcss

# Using Yarn
yarn add tailwindcss
```

## 2. 创建 tailwind 配置文件（可选）

如果你希望定制化tailwind.css， 可以试着引入 tailwind 的配置文件

```
npx tailwindcss init 
```

这会在你的工程根目录创建一个最小的`tailwind.config.js`

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
}
```

## 3. 配置 PostCSS

你需要添加`PostCSS` 选项来让`tailwind.css` 跑起来

```
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {}
  }
}
```

请在你的工程根目录引入如上所示配置，并命名为`postcss.config.js`

## 4. 把 tailwind 引入工程

现在可以创建一个 CSS 文件在 `src/assets/css/styles.css` 并添加

```javascript
@tailwind base;
@tailwind components;
@tailwind utilities;
```

接着在 main.js 中导入到 vue 工程中

```javascript
import '@/assets/css/style.css'
```

一切顺利的话，你的Vue功能就能使用 tailwind.css 了。以上。
