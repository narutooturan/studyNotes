# Git

- 撤销commit
  - revert：建立新的commit，覆盖需要撤销的commit
```bash
git revert {commit}
```
  - rebase：直接抽出需要撤销的commit
```bash
git rebase -i {commit_prev}

# 如果无法push，可以强制push
git push --force origin {branch}
```
  - reset：将需要撤销的commit之后的所有commit文件转为unstage
```bash
git reset {commit_prev}
```

- 撤销本地commit
```bash
git reset HEAD~
git checkout .
```

- 清除当前修改
```bash
git reset --hard
git clean -fd
```

- stash
```bash
git stash list
git stash clear
git stash apply stash@{2}
git stash apply
```

- git add 后取消修改
```bash
git reset HEAD
// 不建议使用：把全部更改的文件都恢复
// git reset –hard HEAD
git checkout .
```

- log
```bash
git log
git log --stat
git log -p file
```

- 修改 config
```bash
git config --global user.name {name}
git config --global user.email {email}
```

- 重命名本地分支
```bash
git branch -m {new_name}
```

- 合并分支
```bash
git checkout {new_branch}
git merge {old_branch}
```

- 查看所有分支
```bash
git branch -a
```

- 拉取远程分支
```bash
# 1
git fetch
git checkout -b {localName} {origin/originName}

# 2
git pull
git checkout -b {originName}

```

- 对比分支
```bash
git diff branch1 branch2
```