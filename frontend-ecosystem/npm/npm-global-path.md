# 全局安装目录

> 在我的 Mac 机器上，没有配置全局安装目录，会出现 npm global install 后找不到命令的情况，因此来记录一下npm 如何进行全局安装目录的配置。

回到用户根目录下再新建一个全局安装的路径

```bash
cd ~
mkdir ~/.npm-global
```

配置npm使用新的路径

```
vim ~/.zshrc
```

在最后一行添加环境变量申明，即可导入 npm global intsall 的全局路径

```
export PATH=~/.npm-global/bin:$PATH
```

使配置生效

```
source ~/.zshrc
```
