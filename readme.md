### Chapter01: 创建版本库
进入某个文件夹: 注意使用的是斜杠“/”，这一点和windows的路径不一样
撒旦打撒打撒啊啊啊啊
`cd /e/5___Study/Python/14___MMRec/jobs/ConformalPrediction`

- 初始化仓库
  
    `git init`
- 添加文件：添加文件指令可以多次执行

    `git add file1.txt` <br>
    `git add file2.txt file3.txt`
  
- 提交更新
  
    `git commit -m "自定义的更新描述"`

- Tips:
    **commit 在git中被称作“快照”，就相当于游戏的存档。**
  
### Chapter02: 创建版本库
- 查看更新：

    `git status` 查看当前回合的修改信息
  
    `git add .` 添加所有修改到暂存区

    `git log -- file_name` 查看某个文件的提交历史，不指定文件会查看所有文件

    `git log -p file_name` 显示每个提交的详细改动

    `git log --stat` 查看每次提交的统计概要

    `git show <commit>` 查看commit序号那一次提交的详细改动，不指定时为最近一次的commit

- 查看所有提交历史：

    `git log` 查看所有提交历史

- 撤销文件的修改（未提交的更改）：

    `git checkout -- file_name` 撤销指定文件的更改

- 删除已添加但未提交的文件：

    `git rm file_name` 从暂存区删除文件

- 查看当前分支及其状态：

    `git branch` 查看本地分支列表
  
    `git branch -a` 查看所有分支，包括远程分支

- 创建并切换到新分支：

    `git checkout -b new_branch_name` 创建并切换到新分支

- 合并分支到当前分支：

    `git merge branch_name` 合并指定分支到当前分支

- 推送分支到远程仓库：

    `git push origin branch_name` 推送指定分支到远程仓库

- 拉取远程分支最新代码：

    `git pull origin branch_name` 拉取远程分支最新代码



  