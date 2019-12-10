# 主题切换

## Props参数下发

提供一个`theme.js`数据文件，包含了`DEFAULT`和`FIRST` 等配置

```javascript
const DEFAULT_THEME = {
  primary: '#2F54EB',
  subPrimary: '#D6E4FF',
  error: '#F5222D',
  success: '#52C41A',
  warning: '#FAAD14',
  background: '#FFFFFF',
  text: '#222222'
}
export default {
    DEFAULT: DEFAULT_THEME,
    FIRST: {
        ...DEFAULT_THEME,
        background: '#2590ff'
    }
}
```

在组件中使用

```javascript
<script>
import theme from 'theme.js'
export default {
    name: "themeChange",
    data() {
        return {
            theme: theme['DEFAULT']
        }
    },
    methods: {
        changeTheme() {
            this.theme = theme['FIRST']
        }
    }
}
</script>
```

## Ajax替换

通过`Ajax`获取`css`，然后替换其中的颜色变量，再通过`style`标签将样式插入`DOM`

> [https://github.com/ALetterSong/Note/issues/15](https://github.com/ALetterSong/Note/issues/15)

## 动态Less

使用可以在浏览器上直接运行的`less`，通过传入变量动态编译。

> [https://pro.ant.design/docs/theme-cn\#%E5%9C%A8%E7%BA%BF%E5%88%87%E6%8D%A2%E4%B8%BB%E9%A2%98](https://pro.ant.design/docs/theme-cn#%E5%9C%A8%E7%BA%BF%E5%88%87%E6%8D%A2%E4%B8%BB%E9%A2%98)

## 

