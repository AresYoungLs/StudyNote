## 基础命令

#### git commit

- Git 提交当前本地仓库代码

### git add .

- 提交当前的文件到临时仓库

### git checkout -b [branchName]

- Git 创建并且切换到分支

### git checkout [branchName]

- Git 切换分支

### git merge [branchName]

- 将[branchName]分支内容合并到当前所在分支

### git rebase [branchName]

- 将当前的分支的工作“复制”到所选[branchName]分支上

> Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去。

> Rebase 的优势就是可以创造更线性的提交历史，如果只允许使用 Rebase 的话，代码库的提交历史将会变得异常清晰。

> 移动以后会使得两个分支的功能看起来像是按顺序开发，但实际上它们是并行开发的。

> 原本的 branchName 分支上的提交记录<font color="#error">依然存在</font> ，而当前分支的提交是我们 <font color="#error">Rebase</font> 到 <font color="#error">master</font> 分支上的 <font color="#error">提交记录的副本</font> 
### 查看git用户名和邮箱地址命令：
- git config user.name
- git config user.email
#### 修改用户名和邮箱地址：
- git config --global user.name "username"
- git config --global user.email "email"
- 当git注册时的邮箱发生变化后，可以通过config命令进行修改。

---

## 高级命令

### HEAD概述

### git show HEAD
- 查看当前的HEAD的父母的消息
- `^` 查看上一个
- `~[num]` 查看上`num`个的父母的消息

### git checkout [commitName]

- 分离 Head
- 将 Head 指向具体的提交而并非是一个分支

> HEAD 是一个对当前检出记录的符号引用 —— 也就是指向你正在其基础上进行工作的提交记录。

> HEAD 总是指向当前分支上最近一次提交记录。大多数修改提交树的 Git 命令都是从改变 HEAD 的指向开始的。

> HEAD 通常情况下是指向分支名的（如 bugFix）。在你提交时，改变了 bugFix 的状态，这一变化通过 HEAD 变得可见。

> 如果想看 HEAD 指向，可以通过 `cat .git/HEAD` 查看， 如果 HEAD 指向的是一个引用，还可以用 `git symbolic-ref HEAD` 查看它的指向。但是该程序不支持这两个命令）

> 分离的 HEAD 就是让其指向了某个具体的提交记录而不是分支

### git checkout HEAD^ / git checkout HEAD~3 / git checkout j25g24v3.....

- Head 相对引用
- `git checkout HEAD^` 将当前的 HEAD 指向向上移动 1 个位置，让 HEAD 指向上一个提交记录
- `git checkout HEAD~3`将当前的 HEAD 指向向上移动(一次后退)3 个位置，让 HEAD 指向从当前的指向<font color="#error">向上移动3次</font>提交记录
- `git checkout j25g24v3.....`将当前的 HEAD 强制指向<font color="#error">j25g24v3.....</font>提交记录
  
> `git log` 来查查看提交记录的哈希值

> Git 对哈希的处理很智能。你只需要提供能够唯一标识提交记录的前几个字符即可。因此我可以仅输入 fed2.... 而不是一长串字符

### git branch -f [branchName] HEAD~3 / git branch -f [branchName] c5qe8qd1..... /  git branch -f [branchName] [branchName2]

- `git branch -f [branchName] HEAD~3`
  - 将[branchName]分支强制指向 <font color="#error">当前 HEAD 位置向上 3 个</font> 的提交记录
- `git branch -f [branchName] c5qe8qd1.....`将[branchName]分支强制指向 <font color="#error">c5qe8qd1...</font> 的提交记录
- `git branch -f [branchName] [branchName2]`将[branchName]分支强制指向 <font color="#error">[branchName2]</font> 的指向位置
### git reset HEAD^ / git revert HEAD

- `git reset HEAD^`
  - 将当前所在的分支指向 HEAD 的上一个提交记录（c1），但是远程仓库的分支指向仍然是未移动之前的记录(c2)，对于本地仓库而言，c2 如同没有提交过一样，在 `reset` 后， C2 所做的变更还在，但是处于未加入暂存区状态；但是对于远程仓库而言，是无效的
- `git revert HEAD` 
    - 将当前所在的分支从 `HEAD` <font color="#error">当前所指向的分支的提交记录的上一份提交记录</font>复制一份过来,变成一个新的提交，但是本次提交其实是和`revert`前的上一份提交记录保持一致的，也就是说这一次操作其实是用来撤销最后一次的提交的
### git pull --allow-unrelated-histories
> 在本地项目提交为git项目的时候，做好git创建的时候，pull会失败，因为本地和远程的关系未建立，先允许拉取历史记录

### git merge master --allow-unrelated-histories
> 合并无关历史
---
## 移动提交记录（整理提交记录）
### Git Cherry-pick  cada1... caada2... daag3...
- 将<font color="#error">一些</font>提交复制到当前所在的位置（HEAD）下面

### git rebase -i HEAD~4
- `交互式 rebase` 选取当前位置向前num个提交记录进行后续操作
- `交互式 rebase` 指的是使用带参数 --interactive 的 rebase 命令, 简写为 -i
- 如果你在命令后增加了这个选项, Git 会打开一个 UI 界面并列出将要被复制到目标分支的备选提交记录，它还会显示每个提交记录的哈希值和提交说明，提交说明有助于你理解这个提交进行了哪些更改。在实际使用时，所谓的 UI 窗口一般会在文本编辑器 —— 如 Vim —— 中打开一个文件。
- 当 rebase UI界面打开时, 你能做3件事: 
  1. 调整提交记录的顺序（通过鼠标拖放来完成）
  2. 删除你不想要的提交（通过切换 pick 的状态来完成，关闭就意味着你不想要这个提交记录）
  3. 合并提交，它允许你把多个提交记录合并成一个。
  
---
## 杂项
### 只取一个提交记录/本地栈式提交
---
## 高级话题