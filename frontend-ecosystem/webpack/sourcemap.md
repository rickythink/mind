# sourcemap

`eval`： 生成代码 每个模块都被eval执行，并且存在@sourceURL

`cheap-eval-source-map`： 转换代码（行内） 每个模块被eval执行，并且sourcemap作为eval的一个dataurl

`cheap-module-eval-source-map`： 原始代码（只有行内） 同样道理，但是更高的质量和更低的性能

`eval-source-map`： 原始代码 同样道理，但是最高的质量和最低的性能

`cheap-source-map`： 转换代码（**行内**） 生成的sourcemap没有列映射，从loaders生成的sourcemap没有被使用

`cheap-module-source-map`： 原始代码（**只有行内**） 与上面一样除了每行特点的从loader中进行映射

`source-map`： 原始代码 最好的sourcemap质量有完整的结果，但是会很慢

看似配置项很多， 其实只是五个关键字`eval`，`source-map`，`cheap`，`module`，`inline`的任意组合。这五个关键字每一项都代表一个特性， 这四种特性可以任意组合。它们分别代表以下五种特性（单独看特性说明有点不明所以，别急，往下看）：

* eval： 使用eval包裹模块代码
* source-map： 产生`.map`文件
* cheap： 不包含列信息（关于列信息的解释下面会有详细介绍\)也不包含loader的sourcemap
* module： 包含loader的sourcemap（比如jsx to js ，babel的sourcemap）
* inline： 将`.map`作为DataURI嵌入，不单独生成`.map`文件（这个配置项比较少见）



