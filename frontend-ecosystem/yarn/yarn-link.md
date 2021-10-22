# yarn link

`yarn link` 可以用作本地npm包的调试

## 使用方法

假设有两个同级文件夹 `react` and `react-relay` ，要在`react-relay` 中使用`react` 包

```
$ cd react
$ yarn link
yarn link vx.x.x
success Registered "react".
info You can now run `yarn link "react"` in the projects where you want to use this module and it will be used instead.
```

```
$ cd ../react-relay
$ yarn link react
yarn link vx.x.x
success Registered "react".
```

这会创建一个 `react-relay/node_modules/react` 软链接到本地 `react` 工程.

链接注册在 `~/.config/yarn/link`.

取消这个任务，则分别在两个文件夹使用 `yarn unlink` 或者 `yarn unlink [package]`.&#x20;

