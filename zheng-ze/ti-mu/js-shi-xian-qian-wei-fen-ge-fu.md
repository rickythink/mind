# JS实现千位分隔符

题目：

```javascript
// given 123456
let input = 123456
// output 123,456
```

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="答案" %}
```javascript
const f = (str) => {
    return str.replace(/(?=(\B\d{3}+$))/g/, ',')
}
```
{% endtab %}

{% tab title="答案2" %}
```javascript
const f = (num) => {
  if(typeof num !== 'number') throw Error('not number input')
  const str = `${num}`
  let count = 0, ret = []
  for(let i=str.length-1; i>=0 ; i--) {
    count++
    ret.unshift(str[i])
    if(count === 3 && i !=0) { 
      ret.unshift(',')
      count = 0
    }
  }
  return ret.join('')
}
```
{% endtab %}
{% endtabs %}

运用到的知识点：

* `g`是表示全局匹配的修饰符，全局匹配指查找所有匹配而非在找到第一个匹配后停止
* `$`是表示结尾的量词，如`n$`，匹配的是任何以n为结尾的字符串
* `\d`是查找数字的元字符
* `n{X}`是匹配包含 X 个 n 的序列的字符串的量词
* `n+`是匹配任何包含至少一个 n 的字符串的量词
* `?=n`正向查找位置，用于匹配任何其后紧接指定字符串 n 的字符串
* `replace()`String对象的方法，作用是替换与正则表达式匹配的子串
* `\B`是表示匹配非单词边界的元字符，与其互为补集的元字符是`\b`，表示匹配单词边界。

