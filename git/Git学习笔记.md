Git学习笔记

=

1. 版本控制
   1. 本地版本控制：个人维护自己的版本，容易混淆，不适用于多人协同操作
   2. 集中化版本控制系统：由服务器统一管理版本，客户端只能拿去单个文件，主要问题是单点故障
   3. 分布式版本控制系统：服务器类似于集中化（远程仓库），本地也保留一份版本记录（本地仓库），提交变更时先修改本地仓库，然后再上传到远程仓库，不同客户端之间可以互相同步
      本地版本库保存在 .git 隐藏目录中
       

1. Git安装完毕后需要配置用户名和邮箱

2. 1. **#      git config --global user.name "Your_name"**
   2. **#      git config --global user.email "Your_email@mail.com"**
   3. **# git config --list**           查看所有配置

3. Git文件的三种状态
              

4. 1. Committed（已提交）：已提交表示数据已经安全保存到本地数据库（本地仓库）中
   2. Modified（已修改）：已修改表示修改了文件，但还没保存到数据库（本地仓库）中
   3. Staged（已暂存）：已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中（修改已放到staging area）

 

工作流程：

**工作区（****working directory****）**  **->**  **暂存区（****staging area****）**  **->**  **本地仓库** **git directory** **（****local repository****）**

​       **^     stage files           commit            |**

​       **|------------------------<checkout the project<------------------------|**

1. 本地仓库操作

2. 1. 初始化git本地仓库（创建空仓库）
                  **# git init**
   2. 在 .git 同级目录下创建新文件 git01.txt
   3. 将文件添加到暂存区
                  **# git add <path>**
   4. 查看工作目录与暂存区文件状态，无法查看已经commit的状态
                  **# git status**
   5. 提交暂存区文件到本地版本库中
                  **# git commit -m 'comment'**
   6. 查看提交日志信息
                  **# git log**
   7. 查看当前HEAD version **./.git/HEAD**
   8. 从暂存区移除文件
                  **# git rm --cached <file>**
                   