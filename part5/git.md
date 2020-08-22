# Git

- stash

```
git stash list
git stash clear
```

- git add 后取消修改

```

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

- 丢弃本地需改

```bash

git checkout .

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