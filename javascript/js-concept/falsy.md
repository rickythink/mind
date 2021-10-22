# Falsy

**falsy **值 (虚值) 是在 [Boolean](https://developer.mozilla.org/en-US/docs/Glossary/Boolean) 上下文中认定为 false 的值。

[JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript) 在需要用到布尔类型值的上下文中使用强制类型转换([Type Conversion](https://developer.mozilla.org/en-US/docs/Glossary/Type\_Conversion) )将值转换为布尔值，例如[条件语句](https://developer.mozilla.org/zh-CN/docs/learn/JavaScript/Building\_blocks/conditionals)和循环语句。

在 JavaScript 中只有 7** 个** **falsy** 值。

这意味着当 JavaScript 期望一个布尔值，并被给与下面值中的一个时，它总是会被当做 false。

| `false`                                                                  | [false](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical\_grammar#Future\_reserved\_keywords\_in\_older\_standards) 关键字                              |   |
| ------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| 0                                                                        | 数值 [zero](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data\_structures#Number\_type)                                                                              |   |
| 0n                                                                       | 当 [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/BigInt) 作为布尔值使用时, 遵从其作为数值的规则. `0n` 是 _falsy _值.                                  |   |
| "", '', \`\`                                                             | 这是一个空字符串 (字符串的长度为零). JavaScript 中的字符串可用双引号 **`""`**, 单引号 `''`, 或 [模板字面量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template\_literals) **` `` `** 定义。 |   |
| [null](https://developer.mozilla.org/en-US/docs/Glossary/null)           | [null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/null) - 缺少值                                                                          |   |
| [undefined](https://developer.mozilla.org/en-US/docs/Glossary/undefined) | [undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/undefined) - 原始值                                                                |   |
| [NaN](https://developer.mozilla.org/en-US/docs/Glossary/NaN)             | [NaN ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/NaN)- 非数值                                                                            |   |
