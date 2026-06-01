---
tags:
  - git
---

## 🌳 Git Branch 核心笔记

### 1. 查看分支 (Viewing Branches)

最常用的功能，用来看看当前仓库都有哪些分支，以及自己目前在哪。

* **查看本地分支：**
`git branch`
*当前所在的分支前面会有一个 `*` 号，并且通常会高亮显示。*
* **查看所有分支（本地 + 远程）：**
`git branch -a`
*`a` 代表 all。红色的通常是远程分支（如 `remotes/origin/main`）。*
* **查看远程分支：**
`git branch -r`
*`r` 代表 remote。*
* **查看带有最后一次提交信息的分支：**
`git branch -v`
*显示每个分支的最后一次 Commit Hash 和提交说明。*

---

### 2. 创建分支 (Creating Branches)

* **基于当前所在位置创建一个新分支：**
`git branch <branch-name>`
> [!NOTE]
> **注意：** 这个命令**只创建，不切换**。创建后你依然停留在原来的分支上。要想创建并立刻过去，还得用 `git checkout -b <name>` 或 `git switch -c <name>`。


* **基于特定的 Commit 创建分支：**
`git branch <branch-name> <commit-hash>`
*非常适合用来找回某个历史版本并从那里重新开始。*

---

### 3. 删除分支 (Deleting Branches)

用完的分支（比如已经合并的 feature 分支）要及时清理，保持仓库整洁。

* **安全删除本地分支：**
`git branch -d <branch-name>`
*`d` 代表 delete。Git 会很聪明地检查：如果这个分支的代码还没被合并到当前分支，它会报错阻止你删除。*
* **强制删除本地分支：**
`git branch -D <branch-name>`
*大写的 `D` 代表 Force Delete。即使代码没合并，也强行丢弃。**危险操作，慎用！***

---

### 4. 重命名分支 (Renaming Branches)

如果你不小心拼错了分支名，或者想把旧的 `master` 改为 `main`。

* **重命名当前所在的分支：**
`git branch -m <new-branch-name>`
*`m` 代表 move/rename。*
* **重命名指定的（非当前）分支：**
`git branch -m <old-name> <new-name>`

---

### 5. 高级用法：仓库大扫除

当项目越来越大时，这些命令能帮你快速梳理需要清理的分支。

* **查看哪些分支已经合并到了当前分支：**
`git branch --merged`
*列出来的这些分支，通常都可以放心用 `-d` 删掉。*
* **查看哪些分支包含还未合并的独立工作：**
`git branch --no-merged`
*列出来的分支如果用 `-d` 删除会报错，需要确认里面的代码是不是还需要。*

---

### 💡 常见组合拳 (Workflow Tips)：

1. **改名并推送到远程（旧分支改名）：**
如果你把本地分支改名了，远程的不会自动改。你需要：
2. 改本地：`git branch -m old new`
3. 删远程旧的：`git push origin --delete old`
4. 推本地新的：`git push origin new -u`


5. **分支不可见问题：**
如果你知道同事刚推了一个新分支到远程，但你用 `git branch -a` 看不到，记得先运行一下 **`git fetch`**，更新本地的远程引用列表。