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


### Chapter07: 分支

##### 7.1 基本概念
- 分支的定义
    
    分支是Git仓库中的一个指针，指向某个提交对象。默认情况下，Git仓库只有一个主分支，通常称为 master 或 main。你可以创建新的分支以便在不影响主分支的情况下进行开发工作。

- 分支的用途
    - 并行开发：分支允许不同的开发人员在不同的功能或修复上并行工作。
    
    - 实验：分支可以用于试验新特性或重大更改，而不影响主分支的稳定性。
    
    - 代码审查和协作：分支可以用于进行代码审查和协作开发，通过Pull Request或Merge Request来合并更改。

##### 7.2 分支相关命令
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
    
- 合并分支到当前分支

    `git merge branch_name` 合并指定分支到当前分支

- 删除分支（一般合并后就可以删除了）

    `git branch -d branch_name`

- 推送分支到远程仓库

    `git push origin branch_name`

- 拉取远程分支最新代码：

    `git pull origin branch_name` 

##### 7.3 分支管理：冲突
    
- 处理方法

    当使用`git merge branch_name`时，如果发生冲突，会提示**自动合并失败**：
    ```bash
    Auto-merging readme.md
    CONFLICT (content): Merge conflict in readme.md
    Automatic merge failed; fix conflicts and then commit the result.
    ```
    此时，md文档中，冲突部分会出现如下标识符：
    ```bash
    <<<<<<< HEAD
    你的内容1
    你的内容2
    你的内容3
    >>>>>>> branch_name
    ```
    修改这段部分的内容为一个标准版本，即可解决冲突，修改完成后，运行如下指令，即可重新完成合并：
    ```bash
    git add .
    git commit -m "commit_message"
    git merge branch_name
    ```

- 查看处理结果

    执行如下命令即可查看合并的过程：
  
    `git log --graph --pretty=oneline --abbrev-commit`

    - `--pretty=oneline`表示一次提交只占一行
    - `--abbrev-commit`表示简短地表示`commit id`
    
    展示结果如下：
    ```bash
    *   ed44217 (HEAD -> master) 06211358_merge
    |\
    | * 39df56a (dev) 06211348_dev
    * | 112b540 06211349_master
    |/
    * 8ed8d04 06211149_master
    ```


##### 7.4 分支管理：分支合并策略

- `Fast forward` 合并

    默认情况下的选择。如果目标分支直接从源分支快进（fast forward），Git会直接将目标分支指向源分支的最新提交。
这种合并方式不会创建新的合并提交，合并历史看起来是线性的。

    - 缺点：合并后删除分支会丢掉分支信息，不利于查看历史上的分支操作。
    
    - 注意：合并后，之前的那个分支就没有意义了，需要及时删除。
    
    - 图示说明
    ```bash
    # [Fast forward合并]初始状态
    A---B---C (master)
           \
            D---E (dev)
    # [Fast forward合并]合并后
    A---B---C---D---E (master, dev)
    ```
    
- `--no-ff` 合并：

    使用 --no-ff 参数强制Git在合并时生成一个新的合并提交，即使可以使用快进合并。
这种方式合并后可以保留分支信息，在合并历史中清晰地显示曾经做过的合并操作。
合并命令示例：
  
  `git merge --no-ff -m "merge with no-ff" dev`

    - 特点：合并后，原始分支仍然会保留，可以继续开发。

    - 图示说明
  
    ```bash
    # [--no-ff合并]初始状态
    A---B---C (master)
           \
            D---E (dev)
    # [--no-ff合并]合并后
    A---B---C------G (master)
         \    /
          D---E (dev)
    ```

##### 7.5 分支管理：BUG修复

本小节将详细介绍在Git中如何高效地处理bug修复，同时保持当前开发工作的连续性。例如：
当你在一个分支上进行开发工作，但需要临时中断去修复一个bug时，可以使用Git的stash功能将当前工作现场“储藏”起来，之后再恢复继续工作。

- 使用`stash`保存工作现场（目前在dev分支）

  `git stash` 保存后，可以使用`git stash list`查看保存的结果。

- 切换到要修复bug的分支（如master）并创建临时分支，修复BUG后提交

  ```bash
  git switch master
  git switch -c issue-101
  git add readme.txt 
  git commit -m "fix bug 101"
  ```
- 合并修复并删除临时分支
  ```bash
  git switch master
  git merge --no-ff -m "merged bug fix 101" issue-101
  git branch -d issue-101
  ```
  
- 还原工作现场
  ```bash
  git switch dev
  git stash pop
  ```
- 其他开发人员：使用`git cherry-pick`命令将修复提交应用到其他分支，避免重复修复工作
  ```bash
  # 4c805e2是之前提交的`commit id`
  git cherry-pick 4c805e2
  ```
  
##### 7.6 分支管理：新功能删除

当需要开发一个新功能时，建议创建一个新的feature分支进行开发，以避免对主分支造成影响。

```bash
git switch -c new_feature  # 开发新模块
git add new_feature.py     # 提交了一个新的代码文件
git commit -m "06211508_newfeature" # 提交
```

此时，需要删除此分支。
```bash
git switch master         # 删除分支前先回到主分支去（也可以去dev）
git branch -D feature-vulcan      # 强行删除
```

##### 7.7 分支管理：多人协作

- 远程分支

  `git remote` 查看远程库的名称
  
  `git remote -v` 查看远程库的详细信息
  
  `git push origin branch_name`把本地分支branch_name推送到远程库
  
  `git switch -c dev origin/dev` 基于远程分支 `origin/dev` 创建一个新的本地分支 `dev`，并切换到这个新的本地分支。对比`git switch -c dev`
  后者是基于本地分支创建一个新的本地分支。

- 远程冲突

  当推送失败时，需要先拉取远程分支的最新提交，解决冲突后再推送：
  ```bash
  git pull    # 拉取远程分支的最新提交
  git add .
  git commit -m "fix conflict"
  git push origin dev
  ```

##### 7.8 分支管理：变基

rebase 的主要作用是将一个分支的更改重新应用到另一个基础提交上。具体来说，它会先从当前分支中提取所有的提交，将其存储在临时区域，然后将当前分支重置到目标基础提交，最后将之前存储的提交依次应用到当前分支上。

下面给一个案例:
```bash
# 假设我们有如下分支结构
A---B---C topic
     \
      D---E master
# 我们希望将 topic 分支的更改重新应用到 master 分支上，使得 topic 分支基于 master 分支的最新提交。
           A'--B'--C' topic
          /
     D---E master
# A'、B' 和 C' 是重新应用到 master 最新提交 E 之后的新提交。

```

### Chapter08 标签

- 切换到需要打标签的分支。

  `git switch dev`

- 给最新的提交打一个标签

  `git tag v1.0`

- 给最新的提交打一个带说明的标签

  `git tag -a <tagname> -m "blablabla..."`

- 给`commit_id`的历史的提交打一个标签

  `git log --pretty=oneline --abbrev-commit` 查看历史commit_id
  
  `git tag v0.9 commit_id`

- 查看当前所有的标签
  
  `git tag`

- 查看某个标签对应版本的详细信息

  `git show tag_name`

- 删除某个本地标签

  git tag -d <tagname>

- 推送一个本地标签

  git push origin <tagname>

### Chapter09 .gitignore文件

忽略某些文件时，需要编写`.gitignore`，`.gitignore`文件本身要放到版本库里，并且可以对.gitignore做版本管理！

这种情况通常是因为 .gitignore 文件可能没有正确配置或提交，或者 .py 文件之前已经被 Git 追踪。下面是详细检查和解决步骤：

1. 确保 .gitignore 文件内容正确
首先，确保你的 .gitignore 文件中确实包含了忽略 .py 文件的规则：

plaintext
复制代码
*.py
2. 确保 .gitignore 文件被正确提交
确保 .gitignore 文件已经被添加并提交到仓库：

bash
复制代码
git add .gitignore
git commit -m "Add .gitignore file to ignore .py files"
3. 移除已经被追踪的 .py 文件
如果 .py 文件之前已经被添加到 Git 暂存区或已经提交，那么需要将它们从 Git 的追踪中移除，然后再提交更改。使用以下命令：

bash
复制代码
# 移除所有已经被追踪的 .py 文件
git rm --cached *.py

# 提交更改
git commit -m "Remove .py files from tracking"
4. 确认 .gitignore 生效
移除文件后，确保新的 .py 文件不再被追踪。你可以创建或修改一个 .py 文件，并使用 git status 检查是否被忽略。

bash
复制代码
# 创建一个新的 .py 文件
echo "print('Hello, World!')" > new_test.py

# 检查 .gitignore 是否生效
git status
5. 提交其他更改
你可以添加和提交其他更改：

bash
复制代码
# 添加所有更改到暂存区
git add .

# 提交更改
git commit -m "Your commit message"

# 推送到远程仓库
git push origin master
完整操作示例
假设你已经正确设置 .gitignore 文件，现在执行以下步骤：

bash
复制代码
# 确保 .gitignore 文件内容正确
echo "*.py" > .gitignore

# 添加并提交 .gitignore 文件
git add .gitignore
git commit -m "Add .gitignore file to ignore .py files"

# 移除已经被追踪的 .py 文件
git rm --cached *.py
git commit -m "Remove .py files from tracking"

# 验证 .gitignore 是否生效
echo "print('Hello, World!')" > new_test.py
git status  # new_test.py 应该显示为未追踪文件（untracked files）

# 添加并提交其他更改
git add .
git commit -m "06211646"
git push origin master
可能的问题和解决方案
.gitignore 文件未提交：确保 .gitignore 文件已经被添加并提交。
文件已被追踪：使用 git rm --cached 移除已经被追踪的文件。
规则写法错误：检查 .gitignore 文件中的规则写法，确保正确。
通过这些步骤，你应该能够确保 .gitignore 文件中的规则生效，并且 .py 文件不会被提交到版本库。
