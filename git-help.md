# GIT Notes

## `git branch` 和 `git checkout`

```bash
git branch -a
git checkout `remotes/origin/branch-name` -b `origin/branch-name` # 切换并创建新本地分支
```

## patch

```bash
git format-patch -1
git apply [name].patch
git am [name].patch
./scripts/checkpatch.pl 0001-loongarch-support-pagesize-12-and-14-bits.patch

git add -u
git commit -s --amend
git push origin HEAD:refs/for/uos-dev-3.1.0
```

## 文件回退到某个版本

```bash
git checkout e85be088dd4023edaa target/loongarch/helper.c
```

## ``git apply``有冲突时

```bash
git am xxx.patch # 尝试直接打入补丁
git apply –reject xxx.patch # 自动合入 patch 中不冲突的代码改动
# solve rej
## find . -name *.rej
## vim *.rej, and then modify sources code of conflict.
vim xxx.rej
git add -u
git  am  --continue
```

## ``git pull``手动修改冲突

```bash
git add -u
git stash
git pull
git stash pop
git status
vim # 修改冲突文件
```

## delete commit

```bash
git log                      # 查询要回滚的 commit_id
git reset --hard commit_id   # HEAD 就会指向此次的提交记录
git push origin HEAD --force # 强制推送到远端
```

## `git log -L`

```bash
git log -L :runOnModule:lib/Analysis/CallGraphSCCPass.cpp
```
