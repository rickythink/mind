# 合并多个commit

文件少的话可以先: `git reset --soft` 然后重新 `git commit`  , 这样又简单又好理解。

 `rebase -i` 合并比较麻烦，利用 rebase 做分支之间的变基会比较好；

下面还是介绍一下rebase的用法：

### 查看版本历史

```text
git log

* b1b8189 - (HEAD -> master) Commit-3
* 5756e15 - Commit-2
* e7ba81d - Commit-1
* 5d39ff2 - Commit-0
```

### 执行git rebase变基

`-i` 实际上就是 `--interactive` 的简写，在使用 `git rebase -i` 时，我们要在后面再添加一个参数，这个参数应该是 **最新的一个想保留的 Commit**。这句话读起来有点坳口，所以这个情况下通常需要举个例子。就我们前面提到的那个例子中，这个「最新的一个想保留的 Commit」就是 `5d39ff2(Commit-0)`，于是我们的命令看起来就长这样：

```text
git rebase -i 5d39ff2
```

请注意5d39ff2这个版本是不参与合并的，可以把它当做一个坐标

### 选择要合并的commit

上一步操作按下回车键后会出现

```text
pick e7ba81d Commit-1
pick 5756e15 Commit-2
pick b1b8189 Commit-3
```

前面三行是我们需要操作的三个 Commit，每行最前面的是对该 Commit 操作的 Command。关于每个 Command 具体做什么，下面的注释写得非常清楚。为了完成我们的需求，我们可以关注到这两个命令：

* `squash`：使用该 Commit，但会被合并到前一个 Commit 当中
* `fixup`：就像 `squash` 那样，但会抛弃这个 Commit 的 Commit message

由于我们是想把三个 Commit 都合并在一起，并且使 Commit Message 写成 Commit-1，所以我们需要把 5756e15\(Commit-2\) 和 b1b8189\(Commit-3\) 前面的 pick 都改为squash，于是它看起来像这样：

```text
pick e7ba81d Commit-1
squash 5756e15 Commit-2
squash b1b8189 Commit-3
```

也可以用缩写

```text
pick e7ba81d Commit-1
s 5756e15 Commit-2
s b1b8189 Commit-3
```

### 保存继续

上一步操作完保存退出后，Git会压缩提交历史，如果有冲突，需要修改，修改的时候要注意，保留最新的历史，不然我们的修改就丢弃了。修改以后要记得敲下面的命令：

```text
git add .  
git rebase --continue  
```

如果要放弃的话，则使用

```text
git rebase --abort
```

### push

推荐使用

```text
git push --force-with-lease
```

{% hint style="info" %}
**git push --force 命令有什么安全问题？**

`--force` 会使用本地分支的提交覆盖远端推送分支的提交。也就是说，如果其他人在相同的分支推送了新的提交，你的这一举动将“删除”他的那些提交！就算在强制推送之前先 `fetch` 并且 `merge` 或 `rebase` 了也是不安全的，因为这些操作到推送之间依然存在时间差，别人的提交可能发生在这个时间差之内。
{% endhint %}

{% hint style="info" %}
**请特别注意**——如果你 `fetch` 之后在本地的 origin 相关分支上已经看到了别人的提交，依然进行强制推送，你还是会覆盖别人的提交。也就是说，`--force-with-lease` **解决的是本地仓库不够新时，依然覆盖了远端新仓库的问题**，如果你**执意想要覆盖远端提交**，只需要先 `fetch` 再推送，**它也不会拒绝的**。
{% endhint %}

{% hint style="info" %}
GitHub 的工作流或者 GitLab 的工作流中，都有一种行为是 `rebase` 自己的分支到 `origin/master` 上，以保证 `master` 分支上的提交是纯粹的干净的。也就是说，本意是禁止对合并到 `master` 或 `develop` 分支上的提交进行 `rebase`；但对于自己的 `temp` 分支或者 `feature` 分支，因为提交还没有合并到主干中，随时删除掉或者将历史进行美化也不会造成太大的问题。
{% endhint %}

