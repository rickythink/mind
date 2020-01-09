# 转换为驼峰命名法

var s1 = "get-element-by-id"; 给定这样一个连字符串，写一个function转换为驼峰命名法形式的字符串 getElementById

{% tabs %}
{% tab title="First Tab" %}
1. 匹配 ' - '
2. 对后面的首字母大写
{% endtab %}

{% tab title="Second Tab" %}
```javascript
const f = (str) => {
    return str.replace( /[-\w\]/g, m => m.slice(1).toUpperCase()
}
```
{% endtab %}
{% endtabs %}

`replace` 的语法

```javascript
str.replace(regexp|substr, newSubstr|function)
```

其中`function` 的回调参数有

| `match` | The matched substring. \(Corresponds to $& a`bove.)` |
| :--- | :--- |
| `p1, p2, ...` | The _n_th string found by a parenthesized capture group, provided the first argument to `replace()` was a [`RegExp`](https://devdocs.io/javascript/global_objects/regexp) object. \(Corresponds to `$1`, `$2`, etc. above.\) For example, if `/(\a+)(\b+)/`, was given, `p1` is the match for `\a+`, and `p2` for `\b+`. |
| `offset` | The offset of the matched substring within the whole string being examined. \(For example, if the whole string was `'abcd'`, and the matched substring was `'bc'`, then this argument will be 1.\) |
| `string` | The whole string being examined. |

