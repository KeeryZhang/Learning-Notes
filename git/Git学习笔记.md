Git学习笔记
=

## 1. 版本控制
1. 本地版本控制：个人维护自己的版本，容易混淆，不适用于多人协同操作
2. 集中化版本控制系统：由服务器统一管理版本，客户端只能拿去单个文件，主要问题是单点故障
3. 分布式版本控制系统：服务器类似于集中化（远程仓库），本地也保留一份版本记录（本地仓库），提交变更时先修改本地仓库，然后再上传到远程仓库，不同客户端之间可以互相同步
> 本地版本库保存在 .git 隐藏目录中


## 2. Git安装完毕后需要配置用户名和邮箱

1. 配置用户名  
`git config --global user.name "Your_name"`
2. 配置邮箱  
`git config --global user.email "Your_email@mail.com"`
3. 查看所有配置  
`git config --list`           


## 3. Git文件的三种状态

>	a. Committed（已提交）：已提交表示数据已经安全保存到本地数据库（本地仓库）中
>	
>	b. Modified（已修改）：已修改表示修改了文件，但还没保存到数据库（本地仓库）中
>	
>	c. Staged（已暂存）：已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中（修改已放到staging area）

 

      工作流程：
    
      工作区（working directory）    ->    暂存区（staging area）    ->    本地仓库 git directory（local repository）
      
      ​           ^             stage files                       commit                 |
      
      ​           |------------------------<checkout the project<------------------------|

## 4. 本地仓库操作

> a. 初始化git本地仓库（创建空仓库）
> 	
>	`git init`
>	
> b. 在 .git 同级目录下创建新文件 git01.txt
> 	
> c. 将文件添加到暂存区
>   	
>	`git add <path>`
>	
> d. 查看工作目录与暂存区文件状态，无法查看已经commit的状态
>   	
>	`git status`
>	
> e. 提交暂存区文件到本地版本库中
>  	
>	`git commit -m 'comment'`
>	
> f. 查看提交日志信息
>   	
> `git log`
> 
> g. 查看当前HEAD version 
>   	
>	`./.git/HEAD`
>	
> h. 从暂存区移除文件
>   	
>	`git rm --cached <file>`

##	5. 文件修改并提交
> a. 查看文件修改情况
>		
> `git status`
> 
> b. 将所有变更文件一次性都添加到暂存区
>		
> `git add .`      或      `# git add <git绝对路径>`
> 
> c. 直接提交
>		
> `git commit` 之后会强制进入comment编辑页面，输入comment后按vi操作方式保存退出
> 
> d. 若修改文件后直接使用commit提交，提交会失败
>		
> e. 使用版本对比命令查看文件修改
```diff
git diff HEAD -- git01.txt
diff --git a/git01.txt b/git01.txt
index 5390518..e7dac62 100644
--- a/git01.txt                       	指变动之前的文件
+++ b/git01.txt                      	指变动之后的文件
@@ -1,2 +1,3 @@                      	变动的区别，-1,2意思是变动前的文件是从第一行开始连续2行，+1,3意思是变动后的文件是从第一行开始连续3行
 content_1                            	第1行未发生变化
-content_2                            	第2行变动（添加换行）
\ No newline at end of file
+content_2                            	新的第2行
+content_3                            	添加的新的第3行
\ No newline at end of file
```

##	6. 暂存区文件的提交与撤销
> a. 提交
> 
> `git commit -m 'comment'`
> 
>	b. 撤销
>	
> `git restore --staged <file>`         仅用于移除add操作
> 
> `git reset HEAD <file>`               回退上一次操作，不仅可以移除add，也能移除commit

## 7. 版本回退
>	a. 跳过add直接提交
>	
> `git commit -am 'comment'`            -a就是add，将当前目录下所有文件都进行add并commit
> 
>	b. 版本回退
>	
> `git reset --hard HEAD^`              一个^代表回退一个版本，需要回退多个版本可以使用多个^
> 
>	c. 回退多个版本
>	
> `git reset --hard HEAD~50`            回退50个版本
> 
> d. 直接转移到某个特定版本
>		
> `git reset --hard 7aa4c87bc9b32`      这一串是每个版本的唯一标识ID，不需要输入全部，只要能保证唯一就可以被识别
> 
> e. 若回退后无法找到最新的版本描述，可以查看所有操作日志
>		
> `git reflog`
> 
> f. reset参数对比
>>	i. `--soft`：  在本地库移动HEAD指针，不会对暂存区和工作区做修改（相当于回退了commit）
>>	
>>	ii. `--mixed`：在本地库移动HEAD指针，重置暂存区，不会修改工作区（相当于回退了commit+add）
>>	
>> iii. `--hard`：  在本地库移动HEAD指针，重置暂存区，重置工作区（相当于回退了commit+add+modify）
>> 
>> ![image](https://user-images.githubusercontent.com/61826668/127801399-5a2ef358-8921-4077-b76b-d9df570ab9bf.png)

##	8. 删除文件
> a. 删除工作区域文件
> 
>	b. 查看git中包含的文件
>	
> `git ls-files`
> 
>	c. 切换本地库中版本（可用于回滚操作）
>	
> `git checkout <version>`
> 
>	d. 查看文件状态（此时删除的文件应该为红色deleted）
>	
> `git status`
> 
>	e. 将删除操作添加到暂存区（再查看status，被删除的文件变为绿色deleted）
>	
> `git add <deleted file>`
> 
>	f. 提交删除操作
>	
> `git commit -m 'comment'`
> 
		
##	9. 第二种删除方式
>	a. 查看本地库文件
>	
> `git ls-files`
> 
>	b. 直接从git中删除，无需提交，且工作区文件会同时被删除
>	
> `git rm 'file'`
> 
		
##	10. 下载远程仓库
>	a. 直接下载ZIP，自行解压
>	
>	b. 下载远程仓库，无需登录，通过https方式
>	
> `git clone <https_url>`
> 
>	c. 通过SSH方式下载，更加快速，安全
>	
>>	i. 生成public ssh key，一路回车
>>	
>> `ssh-keygen -t rsa -C "github_mail"`
>> 
>>	ii. 找到生成的密钥
>>	
>> `Your public key has been saved in <path>`
>> 
>>	iii. 打开公钥 id_rsa.pub，全选复制，添加到
>>	
>>	`GitHub -> Account -> Setting -> SSH and GPG keys -> New SSH key -> Title随意 -> Paste in Key -> Add SSH key`
>>	
>>	iv. 检查是否关联成功，需要手动输入 'yes'
>>	
>> `ssh -T git@github.com`
>> 
>> `ssh -T git@github.west.isilon.com （公司私有github）`
>> 
>>	v. 认证成功
>>	
>> `You've successfully authenticated.`
>> 
>>	vi. 下载远程仓库
>>	
>> `git clone git@github.com:<project_name>.git`
>> 
>	d. clone实现的效果
>	
>>	i. 完整的把远程库下载到本地
>>	
>>	ii. 创建origin远程地址别名
>>	
>> iii. 初始化本地库

##	11. 绑定远程仓库
>	a. 创建远程仓库
>	
> `头像旁边“+”号 -> New repository -> Repository Name -> Create repository`
> 
>	b. License：相当于对本项目权限的规定，是否允许二次开发，是否允许商用，是否允许直接开发
>	
>	c. 切换到master分支，若先进行本地创建，自动选择为master，可以跳过这步
>	
> `git branch -M master`
> 
>	d. 添加远程仓库（将本地仓库与远程仓库关联）
>	
> `git remote add origin git@github.com:<project_name>.git`
> 
>	e. 将已经存在的master推送到远程仓库
>	
> `git push -u origin master`

##	12. 将文件变动从本地仓库推送到远程仓库
>	a. 文件修改（新增，修改，删除）
>	
>	b. 在本地添加到暂存区
>	
> `git add .`
> 
>	c. 本地提交修改
>	
> `git commit -m 'comment'`
> 
>	d. 推送到远程
>	
> `git push`

##	13. 本地分支操作
>	a. 切换到指定分支
>	
> `git checkout branch`
> 
>	b. 新建分支并切换到新建分支，这里创建的new branch是基于master（main）的
>	
> `git checkout -b new_branch`
> 
>	c. 删除指定分支
>	
> `git branch -d branch`
> 
>	d. 若本地和远程分支内容不一致，或已经将远程分支删除，需要强制删除本地分支
>	
> `git branch -D branch`
> 
>	e. 查看所有分支，并且*号标记当前所在分支
>	
> `git branch`
> 
>	f. 合并分支，必须要先切换到主干，然后合并
>	
> `git merge branch`
> 
>	g. 重命名分支，如果newbranch名字的分支已存在，则需要使用`-M`强制重命名，否则，使用`-m`进行重命名
>	
> `git branch -m | -M oldbranch newbranch`
> 
>	h. 在新分支下创建新文件
>	
>	`add并commit`
>	
>	i. 切换到主干然后合并（需要先切换到被合并的分支）
>	
> `git checkout master`
> 
> `git merge <new_branch>`

##	14. 远程分支操作
>	a. 查看本地与远程分支
>	
> `git branch -a`
> 
>	b. 推送本地分支到远程
>	
> `git push origin branch_name`
> 
>	c. 推送本地所有分支到远程
>	
> `git push`
> 
>	d. 删除远程分支（本地分支依然保留）
>	
> `git push origin :remote_branch`
> 
>	e. 拉取远程分支并在本地创建新分支
>	
> `git checkout -b local_branch origin/remote_branch`
> 
>	f. 获取远程仓库最新状态（若远程直接创建分支或进行分支操作，本地没有该分支信息）
>	
> `git fetch`
> 
>	g. 拉去特定远程库
>	
> `git fetch <remote_repository> <remote_branch>`
> 
>	h. 远程分支合并
>	
> `git merge <remote_repository/remote_branch>`
		
##	15. 本地分支操作冲突（这种情况较少见，因为都属于个人本地操作）
>	a. 当多个branch对同一个文件的同一部位做出修改，在合并到master时就会出现冲突（conflict），打开文件会在冲突部分自动添加。且这时的主干会显示为 (`master|MERGING`)
>	
>     <<<<<<< HEAD
>     Master change
>     =======
>     Branch change
>     >>>>>>> Branch
> 
>	b. 若以主干为准，删除分支的冲突内容，重新在master下`add`并`commit`，然后冲突就解决了。
>	
> 同样，也可以用这种方式保留分支内容，或进行编辑，同时保留两个branch的内容。
		
##	16. 多人协同操作冲突
>	a. 这种情况较为多见，多个开发人员拉取同一主干分支进行开发，修改了同一个文件的同一行
>	
>	b. 当第一个用户推送了(push)更改，第二个用户再推送(push)时就会报错推送失败，需要先进行拉取
>	
> `git pull`
> 
>	c. 在本地解决了冲突后，再次`add`，`commit`，以及`push`
>	
>	d. 一般解决冲突原则是谁发现冲突，谁解决冲突
>	
>	e. 避免冲突，在推送之前，先拉取一次最新的代码

##	17. 标签管理
>	a. 新建标签，默认为当前最后一次提交
>	
> `git tag <tag_name>`
> 
>	b. 为某次操作打标签，操作用唯一标识ID代替
>	
> `git tag <tag_name> <operation_id>`
> 
>	c. 添加标签并指定标签描述信息
>	
> `git tag -a <tag_name> -m 'comment'`
> 
>	d. 为某次操作打标签，并指定标签描述信息
>	
> `git tag -a <tag_name> -m 'comment' <operation_id> `
> 
>	e. 查看所有标签
>	
> `git tag`
> 
>	f. 删除一个本地标签
>	
> `git tag -d <tag_name>`
> 
>	g. 推送本地标签到远程
>	
> `git push origin <tag_name>`
> 
>	h. 推送全部未推送过的本地标签到远程
>	
> `git push origin --tags`
> 
>	i. 删除一个远程标签
>	
> `git push origin :refs/tags/<tag_name>`
		
##	18. fetch与pull的区别
> `git fetch`   更新的是将本地的`remote`信息与远程的`remote`信息同步，但`local`工作区的`HEAD`依然是旧的状态（推荐）
> 
> `git pull`    更新的是本地的`local`信息与远程的`remote`信息同步（类似于`fetch`+`merge`），但`remote`信息是旧的状态（不推荐）
> 
> ![image](https://user-images.githubusercontent.com/61826668/127805769-2fc9afc1-80d6-4e3c-b960-6efd1e4628e6.png)

##	19. 查看log的不同方式
>	a. 显示详细的提交历史
>	
> `git log`
> 
>	b. 查看提交记录，只看唯一标识id和comment，跳过多余的author,email和时间信息
>	
> `git log --pretty=oneline`
> 
>	c. 在上面的基础上，精简标识ID，取前面完全不同的位数
>	
>	d. 查看提交记录，以示意图方式显示冲突解决过程
>	
> `git log --graph --pretty=oneline`
> 
>	e. 显示过去的操作记录，HEAD@{3}这里的数字代表若要回滚之前的状态，需要回滚3步。log是内容的变化，reflog还包括了操作的变化
>	
> `git reflog`
> 
>     02fb8a722898 HEAD@{0}: checkout: moving from BR_STAGING to BR_PSCALE_113271
>     8f3ca8df5b1a HEAD@{1}: merge isilon/BR_STAGING: Fast-forward
>     1307bfe89da6 HEAD@{2}: checkout: moving from BR_MAIN to BR_STAGING
>     8f3ca8df5b1a HEAD@{3}: merge isilon/BR_STAGING: Fast-forward
>     fce3bebf8bfc HEAD@{4}: merge isilon/BR_MAIN: Fast-forward
		
##	20. 文件差异比较
>	a. 比较未提交的文件，显示工作区与暂存区的比较
>	
> `git diff <file>`
> 
>	b. 不指定file，则自动比较多个文件
>	
> `git diff`
> 
>	c. 当前分支与本地库中的某个分支进行比较
>	
> `git diff <branch> <file>`
> 
>	d. 与某个分支的所有文件做比较
>	
> `git diff --full-index BR_MAIN`
