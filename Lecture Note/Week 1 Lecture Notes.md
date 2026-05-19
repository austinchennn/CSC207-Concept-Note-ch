# Week 1 Lecture Notes

## I. Software Development Lifecycle (SDLC)

### 1. Software Development Team Roles

- **Developer:** Designs, implements, and tests the software product.
    
- **Product Manager:** Measures success and manages stakeholders.
    
- **Project Manager:** Identifies Use Cases, manages developers, and handles day-to-day communication between developers and everyone else.
    
- **UX Designer:** Designs how the user navigates between screens, nested menus, and how much effort is required to engage with each use case.
    
- **UI Designer:** Designs the look of what the user sees (fonts, contrast, branding, etc.).
    
- **Quality Assurance (QA):** Responsible for making sure the program meets specifications; tests on different browsers, screen sizes, and network conditions; in charge of alpha and beta testing.
    

### 2. Main Steps in the SDLC

- **Planning and Analysis:** Decide if the project is feasible with the available resources and budget.
    
- **Define Requirements:** Decide what the development team is required to do.
    
- **Design:** Decide on tech needs and develop a prototype.
    
- **Development:** Develop the actual product.
    
- **Testing:** Ensure the product works as intended.
    
- **Deployment:** Deliver the product to the end user.
    
- **Maintenance:** Keep the product up-to-date, ensure compatibility with the latest OS versions, and stay current with real-life company policies affecting users.
    
![[截屏2026-05-06 10.53.31.png]]

---
## II. Java Programming

### 3. Structure of a Java Class

A standard Java class consists of:

- **Declaration:** e.g., `public class ClassName`
    
- **Variables**
    
- **Constructor(s)**
    
- **Methods**
    

### 4. Accessibility Modifiers & Packages

- `public`: The rest of the program can access it.
    
- `private`: Only code within the same class can access it.
    
- `default` _(package-protected)_: Only code in the same package can access it.
    
- `protected`: Package-protected, except subclasses also have access.
    

> [!note] Note **Package** 是 `src` 目录下包含代码的文件夹。它与普通文件夹的区别在于：
> - **逻辑身份**：Package 是代码的**命名空间**，用于区分同名类并决定 `default` 和 `protected` 成员的**访问权限**。
> - **代码约束**：内部源码必须在首行使用 `package` 关键字声明路径（如 `package com.util;`）。
> - **本质**：文件夹是物理存储方式，而 Package 是编译器识别的逻辑组织单元。
> 
> ##### Package vs. 文件夹 (Directory)
>
| 特性       | 文件夹 (Directory)     | Package (包)                                              |
| :------- | :------------------ | :------------------------------------------------------- |
| **本质**   | 操作系统用于组织文件的容器。      | 程序的**命名空间 (Namespace)**，用于逻辑分类。                          |
| **声明要求** | 无需在文件内部声明。          | 源代码第一行必须声明 `package com.example;`。                       |
| **访问控制** | 不影响代码的可见性。          | 决定了 **Default (Package-private)** 和 `protected` 成员的访问权限。 |
| **命名规范** | 随意命名（如 `My Files`）。 | 通常采用反向域名格式（如 `org.apache.utils`），全小写。                    |
| **唯一性**  | 同级目录下不能重名。          | 解决类名冲突（不同包下可以存在同名的 `User` 类）。                            |


### 5. equals() vs. ==

- **`==` Operator:** Compares whatever the variable stores.
    
    - _Reference types:_ Compares memory addresses.
        
    - _Primitive types:_ Compares the literal values.
        
- **`equals()` Method:**
    
    - _In the Object class:_ Compares memory addresses (same as `==`).
        
    - _In other classes:_ Either inherits the `Object` version or is overridden to compare specific object data.
        

### 6. Reference Types vs. Primitive Types

- **Reference Types:** All Java classes are subclasses of `Object`. They have constructors, variables, and methods.
    
- **Primitive Types:** Do not have constructors or methods. They take up less memory, allow for quick operations, and store only their values.
    

### 7. Constructor vs. Method

- **Method:** Takes input, processes values, and returns a value (or `void`). Instance methods must be called through a class instance.
    
- **Constructor:** Instructions for the JVM on how to create and populate variables in a new instance. It does not have a return type and creates a new object in memory rather than just returning an alias.
    

---

## III. Version Control and Git

### 8. Why use Version Control?

- **Backup and restore** - because accidents happen
    
- **Synchronization** - multiple people can make changes
    
- **Short term undo** - that last change made things worse?
    
- **Long term undo** - find out when a bug was introduced
    
- **Track changes** - all changes related to a bug fix
    
- **Sandboxing** - try something out without messing up the main code
    
- **Branching and merging** - (better defined sandboxes)
    

### 9. Git File States (git status)

- **untracked:** Has not been added to your repository yet.
    
    - Untracked (未跟踪)：你从未对该文件运行过任何 Git 命令。
        
- **staged:** Added with `git add` since the last commit.
    
    - Staged (已暂存)：已使用 `git add` 命令将该文件的更改添加到下一次提交的准备队列中。
        
- **tracked:** Exists in the repository but hasn't been added to the staging area since the last commit.
    
    - Tracked (已跟踪)：该文件已被提交（Committed）到版本库中。
        
- **dirty/modified:** Changed since the last time it was added to the staging area.
    
    - Dirty/Modified (已修改)：文件自上次暂存（Staged）以来有了新的更改，但这些更改尚未被暂存。
        

### 10. Git Commands

- `clone`: Create a local copy of all subfolders and files from a remote repository.
    
- `pull`: Download changes from the remote repository that were committed by others.
    
- `add`: Prepares a change to a file (like putting a change into an envelope).
    
- `commit`: Saves the staged changes with a name and timestamp (like closing the envelope).
    
- `push`: Sends previously unsent commits to the remote repository (like mailing the envelope).
    
- `branch`: Creates a parallel set of commits to work on features separately.
    
- `merge`: Combines changes from different branches.
    
- `status`: Displays the state of files in your local repository copy.
    
- `checkout`: Allows you to switch to and work with a specified branch.
    

### 11. Git Conflicts

> [!warning] Conflict Resolution
> 
> A conflict occurs when two commits change the same line of code differently.
> 
> - `>>>>>>`, `======`, and `<<<<<<` symbols denote the boundaries between conflicting versions of the code.
>     
> - The string of alphanumeric characters is the commit hash (name) of the conflicting commit.
>     
> - To resolve, you must manually delete these conflict markers and choose which code to keep.
>     

### 12. Basic Workflow

**1. 启动项目**

- **克隆仓库：** 使用 `git clone <url>` 将远程项目下载到本地。
    

**2. 日常开发流程 (Normal Work)**

当你对代码进行修改后，请遵循以下标准步骤：

- **查看状态：** 运行 `git status` 以确认哪些文件确实发生了更改。
    
- **暂存更改：** 使用 `git add file1 file2 file3` 将指定文件的修改准备好放入下一个提交中。
    
- **提交记录：** 运行 `git commit -m "具有意义的提交信息"`，为本次修改添加说明并存入本地仓库。
    
- **推送代码：** 执行 `git push` 将本地提交同步到远程 GitHub 仓库。
    

### 13. 撤回

**1. 撤销工作区（Working Directory）的修改**

如果你还没执行 `git add`，只是想把当前文件的修改删掉，回到上一次 commit 的状态：

- **命令：** `git checkout -- <filename>` 或 `git restore <filename>`
    
- **效果：** 放弃该文件的本地修改，文件状态从 dirty/modified 回到原本的 tracked 状态。
    
- **安全性：** 高。只会影响你指定的文件，不会移动 HEAD 指针。
    

**2. 撤销暂存区（Staging Area）的修改**

如果你已经运行了 `git add`，但还没 commit，想把文件从“信封”里拿出来：

- **命令：** `git reset HEAD <filename>` 或 `git restore --staged <filename>`
    
- **效果：** 将文件从 staged 状态拉回到 dirty/modified 状态。
    
- **安全性：** 极高。你的代码改动还在，只是不再准备提交了。
    

**3. 产生一个新的提交来“抵消”之前的错误 (git revert)**

这是在团队协作（如 CSSA-Web 或 CSC207 小组项目）中最推荐的方法。

- **命令：** `git revert <commit-hash>`
    
- **效果：** Git 会计算出指定 commit 的相反操作，并自动创建一个新的 commit。
    
- **为什么好用：** 它不会修改过去的历史，而是增加一条新的记录。这样当你执行 `git push` 时，不会因为历史冲突被 GitHub 拒绝。
    

**4. 移动指针但不删除代码 (git reset --soft)**

如果你想撤销最近的一次 commit，但想保留写好的代码以便修改后重新提交：

- **命令：** `git reset --soft HEAD~1`
    
- **效果：** HEAD 指针回退一步，但你之前的改动会全部留在 staged 状态。
    
- **对比：** 这比 `--hard` 安全得多，因为它不会直接抹除你的硬盘数据。
    

### 13. git clone V.S. git pull

`git clone` 是“从无到有”，而 `git pull` 是“同步更新”。

**1. git clone：获取整个项目**

当你第一次在 MacBook M4 Max 上开始一个新项目（比如克隆老师下发的作业模板或参与 CSSA-Web 开发）时，你需要使用这个命令。

- **操作对象：** 一个你本地电脑上还不存在的仓库。
    
- **执行结果：**
    
    - 在本地创建一个与远程仓库同名的文件夹。
        
    - 下载所有的文件、分支和完整的版本历史记录。
        
    - 自动配置名为 `origin` 的远程连接。
        
- **使用频率：** 每个项目通常只执行一次。
    

**2. git pull：更新现有代码**

当你已经克隆了项目，并且正在进行日常开发（Normal Work）时，你需要使用这个命令来获取队友提交的新代码。

- **操作对象：** 一个你本地已经存在且已配置好的 Git 仓库。
    
- **执行结果：**
    
    - 检查远程仓库（`origin`）是否有你本地没有的新提交。
        
    - 下载这些新提交并立即合并到你当前的分支中。
        
- **使用频率：** 非常频繁，建议每天开始写代码前都执行一次，以减少冲突。
    

**3. 核心对比表**

|**特性**|**git clone**|**git pull**|
|---|---|---|
|**前提条件**|本地没有该文件夹|本地已有该仓库且关联了远程地址|
|**下载内容**|整个仓库（含所有历史和分支）|仅下载当前分支缺失的最新提交|
|**自动合并**|否（直接创建新环境）|是（下载后立刻尝试合并）|
|**命令示例**|`git clone <url>`|`git pull` (或 `git pull origin main`)|

---

> [!important] 14. 【关键考点】git commit 但不 git add
> 
> **git commit 但不 git add，就不会保存改变。**
> 
> 解释：如果你跳过了 add 直接 commit，暂存区是空的，本地仓库也就不会记录你刚才在文件里写的代码

> [!important] 15. 【关键考点】Working copy V.S. Local repo
> 
> **1. 核心四层架构 (The 4-Tier Architecture)**
> 
> Git 的数据流转建立在四个核心区域之上：
> 
> - **Origin (远程仓库):** 托管在服务器端（如 GitHub）的版本库。
>     
> - **Local Repo (本地仓库):** 位于本地 `.git` 目录。它存储的是不可变的历史快照，由 Git 底层管理，禁止也无法直接手动修改。
>     
> - **Staging Area / Index (暂存区):** 【核心枢纽】 一个虚拟的缓冲区域。它保存着你准备在下一次提交中包含的文件快照列表。
>     
> - **Working Copy (工作区):** 开发者在电脑上实际能看到、能自由编辑代码的地方。
>     
> 
> **2. 核心流转机制：你的修改并不会直接修改Local repo**
> 
> Git 的设计哲学是“修改”与“记录”严格解耦。
> 
> - 因为 Local Repo 是历史档案库，你不能直接把草稿塞进去。
>     
> - 必须通过一下特定命令，让数据在 **工作区 -> 暂存区 -> 本地仓库 （Working Copy -> Staging Area -> Local Repo)** 之间单向流转，以此生成新的不可变快照。
>     
> 
> **3. 标准流水线与底层本质 (Standard Pipeline)**
> 
> - **编辑 (Edit)**
>     
>     - **操作:** 在 Working Copy 中新增或修改文件（例如：新增 file1, file2）。
>         
>     - **状态:** 此时修改仅存在于物理硬盘上，Git 仓库对这些改变一无所知（Untracked / Modified）。
>         
> - **暂存 (Stage)**
>     
>     - **操作:** 执行 `git add file1 file2`
>         
>     - **本质:** 这是最关键的一步。Git 会读取这些文件的当前内容，将其打包成二进制对象，并更新到 Staging Area (暂存区) 中。此时它们变成了“待提交”状态。
>         
> - **提交 (Commit)**
>     
>     - **操作:** 执行 `git commit`
>         
>     - **本质:** Git 会直接将当前 Staging Area 里的完整状态“拍一张快照”，并将其永久固化到 Local Repo 中。
>         
> 
> **【补充】Push到Branch Local Repo -> Origin**
> 
> - **操作:** 执行 `git push origin <branch_name>`
>     
> - **本质:** 将本地仓库中新生成的、尚未同步的 Commit 快照（以及相关的二进制对象）传输并合并到服务器端的对应分支中。
>