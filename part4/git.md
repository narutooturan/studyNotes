# Git

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