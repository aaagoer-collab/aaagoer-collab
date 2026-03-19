# Git 命令总结

> 本文总结了日常开发中最常用的 Git 命令，帮助你更高效地管理代码版本。

## 目录

- [基础配置](#基础配置)
- [仓库操作](#仓库操作)
- [文件操作](#文件操作)
- [提交历史](#提交历史)
- [分支管理](#分支管理)
- [远程仓库](#远程仓库)
- [撤销操作](#撤销操作)
- [标签管理](#标签管理)
- [储藏操作](#储藏操作)
- [高级技巧](#高级技巧)

---

## 基础配置

### 配置用户信息
```bash
# 配置用户名
git config --global user.name "Your Name"

# 配置邮箱
git config --global user.email "your.email@example.com"

# 查看配置
git config --list

# 查看特定配置
git config user.name
```

### 配置编辑器
```bash
# 配置默认编辑器（以 VS Code 为例）
git config --global core.editor "code --wait"

# 配置为 Vim
git config --global core.editor vim
```

### 配置别名（Aliases）
```bash
# 常用别名配置
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

---

## 仓库操作

### 创建仓库
```bash
# 初始化新仓库
git init

# 克隆远程仓库
git clone <repository-url>

# 克隆指定分支
git clone -b <branch-name> <repository-url>

# 克隆到指定目录
git clone <repository-url> <directory-name>
```

### 查看仓库状态
```bash
# 查看工作区状态
git status

# 简洁状态显示
git status -s

# 查看详细差异
git diff

# 查看已暂存的差异
git diff --cached
```

---

## 文件操作

### 添加文件到暂存区
```bash
# 添加单个文件
git add <file-name>

# 添加所有修改的文件
git add .

# 添加所有修改和删除的文件
git add -A

# 交互式添加
git add -i

# 添加部分修改（交互式）
git add -p
```

### 提交更改
```bash
# 提交暂存区的文件
git commit -m "提交信息"

# 提交并添加所有修改的文件（跳过 git add）
git commit -am "提交信息"

# 修改最后一次提交
git commit --amend -m "新的提交信息"

# 修改最后一次提交但不修改提交信息
git commit --amend --no-edit
```

### 删除和重命名文件
```bash
# 删除文件
git rm <file-name>

# 删除已暂存但未提交的文件
git rm --cached <file-name>

# 强制删除
git rm -f <file-name>

# 重命名文件
git mv <old-name> <new-name>
```

---

## 提交历史

### 查看日志
```bash
# 查看提交历史
git log

# 简洁显示
git log --oneline

# 图形化显示分支历史
git log --graph --oneline --all

# 查看最近 N 条提交
git log -n 5

# 查看某个文件的修改历史
git log -p <file-name>

# 查看提交统计信息
git log --stat

# 查看指定作者的提交
git log --author="Author Name"

# 查看指定日期的提交
git log --since="2024-01-01" --until="2024-12-31"
```

### 查看特定提交
```bash
# 查看某次提交的详细信息
git show <commit-hash>

# 查看某次提交中某个文件的内容
git show <commit-hash>:<file-name>

# 查看某行代码的最后修改者
git blame <file-name>
```

---

## 分支管理

### 查看分支
```bash
# 列出本地分支
git branch

# 列出远程分支
git branch -r

# 列出所有分支
git branch -a

# 查看分支合并状态
git branch --merged
git branch --no-merged
```

### 创建和切换分支
```bash
# 创建新分支
git branch <branch-name>

# 切换分支
git checkout <branch-name>

# 创建并切换到新分支
git checkout -b <branch-name>

# 基于远程分支创建本地分支
git checkout -b <branch-name> origin/<branch-name>

# 创建空分支（无父提交）
git checkout --orphan <branch-name>
```

### 合并分支
```bash
# 合并指定分支到当前分支
git merge <branch-name>

# 禁用快进合并
git merge --no-ff <branch-name>

# 压缩合并（将多个提交合并为一个）
git merge --squash <branch-name>

# 中止合并
git merge --abort
```

### 变基操作
```bash
# 将当前分支变基到主分支
git rebase main

# 交互式变基（修改提交历史）
git rebase -i HEAD~3

# 继续变基（解决冲突后）
git rebase --continue

# 跳过当前提交
git rebase --skip

# 中止变基
git rebase --abort
```

### 删除分支
```bash
# 删除已合并的本地分支
git branch -d <branch-name>

# 强制删除分支
git branch -D <branch-name>

# 删除远程分支
git push origin --delete <branch-name>
```

---

## 远程仓库

### 查看远程仓库
```bash
# 列出远程仓库
git remote -v

# 查看远程仓库详细信息
git remote show origin
```

### 添加和修改远程仓库
```bash
# 添加远程仓库
git remote add origin <repository-url>

# 修改远程仓库地址
git remote set-url origin <new-url>

# 重命名远程仓库
git remote rename old-name new-name

# 删除远程仓库
git remote remove origin
```

### 推送和拉取
```bash
# 推送到远程仓库
git push origin <branch-name>

# 推送所有分支
git push --all origin

# 强制推送（谨慎使用）
git push -f origin <branch-name>

# 推送并建立追踪关系
git push -u origin <branch-name>

# 拉取远程更新
git pull origin <branch-name>

# 拉取但不合并
git fetch origin

# 拉取所有远程分支
git fetch --all

# 拉取并变基
git pull --rebase origin <branch-name>
```

---

## 撤销操作

### 撤销工作区修改
```bash
# 撤销单个文件的修改
git checkout -- <file-name>

# 撤销所有文件的修改
git checkout .

# 恢复文件到指定版本
git checkout <commit-hash> -- <file-name>
```

### 撤销暂存区
```bash
# 取消暂存单个文件
git reset HEAD <file-name>

# 取消暂存所有文件
git reset HEAD .
```

### 回退版本
```bash
# 软回退（保留工作区和暂存区）
git reset --soft HEAD~1

# 混合回退（保留工作区，清空暂存区）
git reset --mixed HEAD~1

# 硬回退（清空工作区和暂存区）⚠️ 危险操作
git reset --hard HEAD~1

# 回退到指定提交
git reset --hard <commit-hash>

# 查看所有操作记录（用于找回）
git reflog
```

### 撤销提交
```bash
# 撤销最后一次提交（创建新提交）
git revert HEAD

# 撤销指定提交
git revert <commit-hash>

# 撤销合并提交
git revert -m 1 <merge-commit-hash>
```

---

## 标签管理

### 创建标签
```bash
# 创建轻量标签
git tag <tag-name>

# 创建附注标签
git tag -a <tag-name> -m "标签说明"

# 给指定提交创建标签
git tag -a <tag-name> <commit-hash> -m "标签说明"
```

### 查看标签
```bash
# 列出所有标签
git tag

# 查看标签详情
git show <tag-name>

# 按模式搜索标签
git tag -l "v1.*"
```

### 推送和删除标签
```bash
# 推送单个标签到远程
git push origin <tag-name>

# 推送所有标签
git push origin --tags

# 删除本地标签
git tag -d <tag-name>

# 删除远程标签
git push origin --delete <tag-name>
```

---

## 储藏操作

### 储藏修改
```bash
# 储藏当前修改
git stash

# 储藏并添加说明
git stash save "储藏说明"

# 储藏包括未跟踪的文件
git stash -u

# 储藏包括忽略的文件
git stash -a
```

### 查看和管理储藏
```bash
# 查看储藏列表
git stash list

# 查看储藏详情
git stash show
git stash show -p

# 应用最近一次储藏
git stash apply

# 应用指定储藏
git stash apply stash@{n}

# 应用并删除储藏
git stash pop

# 删除储藏
git stash drop stash@{n}

# 清空所有储藏
git stash clear
```

---

## 高级技巧

### 子模块管理
```bash
# 添加子模块
git submodule add <repository-url> <path>

# 初始化子模块
git submodule init

# 更新子模块
git submodule update

# 递归克隆包含子模块
git clone --recursive <repository-url>
```

### 归档和打包
```bash
# 创建归档文件
git archive -o latest.zip HEAD

# 创建指定格式的归档
git archive --format=tar HEAD | gzip > latest.tar.gz
```

### 清理仓库
```bash
# 清理未跟踪的文件（预览）
git clean -n

# 强制清理未跟踪的文件
git clean -f

# 清理未跟踪的目录
git clean -fd

# 清理被忽略的文件
git clean -fx
```

### 查找和搜索
```bash
# 在提交历史中搜索
git log --all --grep="关键词"

# 在代码中搜索
git grep "搜索内容"

# 查找包含某内容的提交
git log -S "代码内容" --pretty=format:'%h %s'
```

### 工作树（Worktree）
```bash
# 添加新的工作树
git worktree add <path> <branch-name>

# 列出所有工作树
git worktree list

# 移除工作树
git worktree remove <path>
```

---

## 常用工作流

### 功能分支工作流
```bash
# 1. 创建功能分支
git checkout -b feature/new-feature

# 2. 开发和提交
git add .
git commit -m "Add new feature"

# 3. 保持与主分支同步
git checkout main
git pull origin main
git checkout feature/new-feature
git rebase main

# 4. 推送功能分支
git push -u origin feature/new-feature

# 5. 创建 Pull Request 并合并
# 6. 删除本地功能分支
git branch -d feature/new-feature
```

### 修复紧急 Bug
```bash
# 1. 储藏当前工作
git stash

# 2. 切换到主分支并更新
git checkout main
git pull origin main

# 3. 创建修复分支
git checkout -b hotfix/bug-fix

# 4. 修复并提交
git add .
git commit -m "Fix critical bug"

# 5. 推送到远程
git push origin hotfix/bug-fix

# 6. 恢复之前的工作
git checkout previous-branch
git stash pop
```

---

## 总结

掌握这些 Git 命令可以大大提高你的开发效率。建议：

1. **日常使用**：重点掌握 `add`、`commit`、`push`、`pull`、`branch`、`checkout`、`merge` 等基础命令
2. **团队协作**：熟悉 `rebase`、`cherry-pick`、`stash` 等高级命令
3. **安全第一**：谨慎使用 `--force`、`-f`、`--hard` 等强制操作
4. **保持学习**：Git 功能强大，持续学习新的技巧和最佳实践

> 💡 **提示**：使用 `git help <command>` 可以查看任何命令的详细帮助文档。

---

*最后更新：2026-03-19*
