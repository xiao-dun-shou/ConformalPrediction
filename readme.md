### Chapter01: 创建版本库
进入某个文件夹: 注意使用的是斜杠“/”，这一点和windows的路径不一样

`cd /e/5___Study/Python/14___MMRec/jobs/ConformalPrediction`

- 初始化仓库
  
    `git init`
- 查看当前回合的修改信息
  
  `git status`
- 添加文件：添加文件指令可以多次执行

    `git add file1.txt` <br>
    `git add file2.txt file3.txt` <br>
    `git add .` 添加当前回合的所有修改到暂存区
  
- 提交更新
  
    `git commit -m "自定义的更新描述"`

**在git中 `commit` 被称作“快照”，就相当于游戏的`存档`。**
  
### Chapter02: 创建版本库
- 查看更新

    `git log -- file_name` 查看某个文件的提交历史，不指定文件会查看所有文件

    `git log -p file_name` 显示每个提交的详细改动

    `git log --stat` 查看每次提交的统计概要

    `git show <commit>` 查看commit序号那一次提交的详细改动，不指定时为最近一次的commit

- 回滚历史版本
  
  以下命令能够让搭建仓库的状态回滚到上一个版本。有多少个`^`，就是回滚到第几个版本，如果数值比较大。`HEAD~X`就是回滚到之前X个版本。有必要知道的是，回滚之后，之前的最新版本就不能通过`git log`找到了。如果想要撤回回滚，只要能够找到之前的`commit id`就可以。`commit id`不用完整输入，输入前面几位就会自动完成匹配。从本质来看，就是在修改`HEAD`指针，你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。
  
    `git reset --hard HEAD^` 
  
    `git reflog` 该命令能够查看所有历史命令，以便查看历史的`commit id`。

### Chapter03: 工作区和暂存区

- 工作区（Working Directory）：

  工作区是你在电脑中可以看到和编辑的目录。它包含了你的项目文件。你在文件中所做的修改都是在工作区中进行的。

- 版本库（Repository）

  版本库包含一个隐藏目录 `.git`，这个目录包含了Git用来存储管理版本的所有数据。版本库包括：
  - 暂存区（Stage/Index）：暂存区是一个临时保存区域，它存储了你当前想要提交的文件的快照。
  - 分支（Branch）：默认分支是master，这是你的项目的主要分支。
  - HEAD：指向当前所在的分支，通常指向master。
  
- 工作区和版本库的关系
  
  `git add`命令实际上就是把工作区（Working Directory）要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支（Branch）。

- `.git`目录中的文件

| 文件/文件夹       | 作用                                                                 | 关系                                           |
|-------------------|----------------------------------------------------------------------|------------------------------------------------|
| hooks             | 包含钩子脚本的示例，可以在特定操作时触发                             | 与操作自动化相关                               |
| info              | 包含全局忽略规则的 `exclude` 文件                                    | 控制全局忽略规则                               |
| logs              | 记录对分支的所有操作历史，用于查看和恢复历史操作                     | 记录历史操作                                   |
| objects           | 存储所有的数据对象，包括提交对象、树对象和 Blob 对象                  | 核心数据存储，包含所有版本历史                 |
| refs              | 存储指向提交对象的指针，包括分支、远程分支和标签                      | 管理分支和标签                                 |
| COMMIT_EDITMSG    | 存储最后一次提交的提交信息                                           | 用于提交信息自动填充                           |
| config            | 仓库的配置文件，包含远程仓库地址、分支设置等                         | 仓库的配置选项                                 |
| description       | 描述文件，主要用于裸仓库，描述仓库的用途                             | 用于描述裸仓库                                 |
| HEAD              | 指向当前检出的分支的最新提交                                         | 指向当前分支                                   |
| index             | Git 的暂存区，存储将要提交的文件的快照                               | 表现为暂存区                                   |
| ORIG_HEAD         | 在复杂操作之前保存原来的 HEAD，用于恢复或对比                       | 保存原来的 HEAD                                |

- `.git`目录中的文件与暂存区、分支和 HEAD 的关系

| 名称              | 关系                                                                 |
|-------------------|----------------------------------------------------------------------|
| 暂存区（Stage/Index） | 表现为 `.git/index` 文件，当你执行 `git add` 时，文件的快照被保存到这里 |
| 分支（Branch）    | 表现为 `.git/refs/heads/` 目录中的文件，每个文件代表一个分支          |
| HEAD              | 表现为 `.git/HEAD` 文件，指向当前检出的分支                          |

### Chapter04: 修改

- 查看工作区和版本库里面最新版本的区别
    
    `git diff HEAD -- readme.txt`

  如果你需要提交第二次修改，可以先用 `git add` 暂存第一次修改，然后再用 `git add` 暂存第二次修改，最后用 `git commit` 一次性提交所有修改。这样，两次修改会被合并成一次提交。你也可以选择先提交第一次修改，再提交第二次修改，分别用 `git add` 和 `git commit` 提交每次修改。

  第一次修改 -> `git add `-> 第二次修改 -> `git add` -> `git commit`
- 撤销文件的修改：

  `git checkout -- file_name` 
  
  撤销指定文件的更改，同样可以用`.`来特指全部文件。 换句话说，命令`git checkout -- readme.md`意思就是，把`readme.md`文件在工作区的修改全部撤销，这里有两种情况：

  - case1: `readme.md`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态

  - case2: `readme.md`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区时的状态。
  对于这种情况，除了这种方法，还可以使用`reset`语句。
    `git reset HEAD file_name`
  
  总之，就是让这个文件回到最近一次git commit或git add时的状态。

- 删除已添加但未提交的文件：

    一般来说，有两种方法：
  
  - `git rm file_name` → `git commit -m "描述1"`
  
  - `rm test.txt`(或者在文件管理器中删除) → `git add test.txt` → `git commit -m "描述2"`

  如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

### Chapter05: 添加远程仓库

  你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。
  假设你已经建立好了一个GitHub的仓库，现在，请执行下面的内容完成同步：

  - 把一个已有的本地仓库与Github的仓库关联

    origin是习惯性命名，公开的潜规则，表示这是远程仓库。
    
    `git remote add origin https://github.com/<你的账户名>/<你的仓库名>.git`

  - 第一次推送

    `git push -u origin master` 第一次推送会弹出授权窗口，会花费比较长的时间。

  - 后续推送

    `git push origin master`

  - 删除关联

    `git remote -v` 查看远程库信息。

    `git remote rm origin` 根据远程库的名字删除连接。

  
### Chapter06: 从远程仓库克隆

  想要复制别人的仓库，需要执行如下步骤：

1)  获取远程仓库的URL、你可以从GitHub、GitLab或其他Git托管服务的仓库页面上找到该URL。它通常有HTTPS和SSH两种格式：

    - HTTPS：`https://github.com/username/repository.git`

    - SSH：`git@github.com:username/repository.git`
  
2) 打开终端或Git Bash

    在你的计算机上打开一个终端窗口（在Windows上通常使用Git Bash）

3) 导航到目标目录

    使用`cd`命令导航到你希望存放克隆仓库的目录。例如：

    `cd /path/to/your/directory`

4) 克隆远程仓库

    使用`git clone`命令克隆远程仓库。根据你的远程仓库URL选择合适的格式：

    - 使用HTTPS: `git clone https://github.com/username/repository.git`
      
    - 使用SSH: `git clone git@github.com:username/repository.git`
  


5. 进入克隆的仓库目录：

    克隆完成后，使用`cd repository`导航到克隆的仓库目录。

6. 验证克隆结果：

    可以使用`git status`命令来验证仓库的状态，确保仓库已经正确克隆到本地。

注意：如果你使用HTTPS克隆私有仓库，你可能需要输入用户名和密码。如果你使用SSH克隆私有仓库，请确保你的SSH密钥已添加到Git托管服务


### 分支

##### 分支的定义
分支是Git仓库中的一个指针，指向某个提交对象。默认情况下，Git仓库只有一个主分支，通常称为 master 或 main。你可以创建新的分支以便在不影响主分支的情况下进行开发工作。

##### 分支的用途
- 并行开发：分支允许不同的开发人员在不同的功能或修复上并行工作。

- 实验：分支可以用于试验新特性或重大更改，而不影响主分支的稳定性。

- 代码审查和协作：分支可以用于进行代码审查和协作开发，通过Pull Request或Merge Request来合并更改。

##### 分支相关命令
- 查看当前分支及其状态：

    `git branch` 查看本地分支列表，当前分支前面会标一个`*`号。
  
    `git branch -a` 查看所有分支，包括远程分支

- 创建并切换到新分支：

    `git checkout -b new_branch_name` 
  
    `git switch -c new_branch_name`
  
    上面两条命令都可以创建并切换到新分支，命令等价于下面的命令组合使用：
  
    - `git branch new_branch_name` 创建分支
    - `git checkout new_branch_name` 切换分支
    - `git switch new_branch_name` 切换分支 [最新版本]
    
- 合并分支到当前分支：

    `git merge branch_name` 合并指定分支到当前分支

- 删除分支

    `git branch -d branch_name`

- 推送分支到远程仓库

    `git push origin branch_name`

- 拉取远程分支最新代码：

    `git pull origin branch_name` 

##### 分支冲突
- 冲突标记格式：
     ```text
     <<<<<<< HEAD
     Creating a new branch is quick & simple.
     =======
     Creating a new branch is quick AND simple.
     >>>>>>> feature1
     ```
   - 手动编辑冲突文件并保存：
     ```text
     Creating a new branch is quick and simple.
     ```
   - 标记冲突解决并提交更改：
     ```bash
     git add readme.txt
     git commit -m "conflict fixed"
     ```



  