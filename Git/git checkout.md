---
aliases:
  - git
cover: "[[截屏2026-05-06 10.53.31.png]]"
---

---

## 🛠️ Git Checkout 核心笔记

### 1. 分支管理 (Branching)

这是 `checkout` 最核心的用途。

* **切换分支：**
`git checkout <branch-name>`
*将 HEAD 指向目标分支，并更新工作目录。*
* **创建并立即切换分支：**
`git checkout -b <new-branch-name>`
*这是 `git branch` 和 `git checkout` 的组合拳。*
* **快速切回上一个分支：**
`git checkout -`
*类似于电视遥控器的“回看”键，在两个分支间高频切换时神器。*

---

### 2. 文件恢复 (File Recovery)

如果你改乱了代码，想让某个文件回到最近一次提交的状态：

* **撤销单文件修改：**
`git checkout -- <file-path>`
* **撤销当前目录下所有修改：**
`git checkout -- .`

> [!WARNING]
> **危险操作：** 这种撤销是**不可逆**的。所有未提交（commit）的内容都会被永久覆盖。

---

### 3. 穿越历史 (Exploring History)

你可以让你的代码库倒退回特定的历史时刻。

* **查看特定的提交（Commit）：**
`git checkout <commit-hash>`
*此时会进入 **Detached HEAD**（游离头指针）状态。*
* **从特定分支检出单个文件：**
`git checkout <branch-name> -- <file-path>`
*非常有用的技巧：如果你在 `main` 分支，但想要 `develop` 分支里的某个配置文件，直接用这个命令“偷”过来。*

---

### 4. 命令对照表：旧 vs 新

Git 2.23 版本后，官方建议逐步替代 `checkout` 以减少误操作。

| 任务        | 旧命令 (`checkout`)           | 新命令 (`switch`/`restore`)      |
| --------- | -------------------------- | ----------------------------- |
| 切换分支      | `git checkout <branch>`    | `git switch <branch>`         |
| 创建并切换分支   | `git checkout -b <branch>` | `git switch -c <branch>`      |
| 撤销文件修改    | `git checkout -- <file>`   | `git restore <file>`          |
| 暂存区恢复到工作区 | `git checkout -- <file>`   | `git restore --staged <file>` |

---

### 💡 进阶 Tips：

1. **Tab 补全：** 在终端输入 `git checkout` 后按两下 `Tab`，它会列出所有本地和远程分支。
2. **强制切换：** 如果你有未提交的改动但非要切换分支（且不想 stash），可以使用 `-f` 参数，但要注意这会丢弃你的改动。
3. **冲突处理：** 如果切换分支时提示冲突，Git 会阻止你切换。此时要么 `git commit`，要么 `git stash` 暂存改动。

---

**一句话总结：**

* 想换个**分支**干活？`git checkout <name>`
* 想写个**新功能**？`git checkout -b <name>`
* 想**删掉**刚写烂的代码？`git checkout -- .`