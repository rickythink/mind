# npx

## npx 是什么

npx 是一个工具，它是 npm v5.2.0 引入的一条命令，是 npm 的一个包执行器。

npx 让 npm 包中的命令行工具和其他可执行文件在使用上变得更加简单。

## 例子

### 不使用 npx

```
npm install -g create-react-app
create-react-app test-app
```

### 使用 npx&#x20;

```
npx create-react-app test-app
```

{% hint style="info" %}
npm他会在本地全局性的安装create-react-app，这个包会存储在node目录下面去。以后创建react项目直接执行create-react-app命令就可以了。

npx命令他会把create-react-app安装包临时安装上，等项目初始化完成以后，他就删除掉
{% endhint %}
