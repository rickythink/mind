# 覆盖已有提交

覆盖线上已经提交的commit

```bash
git reset --soft [过去某个commithash]
... （做出修改)
git add (fix)
git commit -m 'fix'
git push -f
```

