#  Git的简单使用

## why Git?	

- `SVN`的**安全控制**和**权限管理**更好。`git`下，如果一个人clone以后，所有代码和历史都泄漏了。而 svn 有细致的**按照目录级的权限控制** (为什么还有那么多人用SVN?) 

- [蒋鑫：为什么 Git 比 SVN 好](http://blog.jobbole.com/20069/)

- [为什么要用GIT而不是SVN](http://www.cnblogs.com/wi100sh/p/4191080.html)
				
## Git base operation

- `git init [project-name]` 	新建一个目录，将其初始化为Git代码库 
	- **工作区(Working Directory):** 当前 `.git` 根目录,除 `.git`
	- **版本库(Repository)** 工作区有⼀一个隐藏⽬目录`.git`
		- stage(或者叫index)的暂存区:**git add”把⽂文件添加进去，实际上就是把⽂文件修改添加到暂存区**

	
- `git add [name]` 	添加当前目录的所有文件到暂存区 (和svn的不同)
- `git status `		显示有变更的文件 
- `git commit -m "change message"` 	提交日志 **实际上就是把暂存区的所有内容提交到当前分⽀支**
- `git commit --amend -m "again adjust change message"`	如果代码没有任何新变化，则用来改写上一次commit的提交信息
- `git checkout [filename]` 把filename文件在工作区的修改全部撤销
	- 一种是filename自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库⼀模⼀样的状态;
	- 一种是filename已经添加到暂存区后，⼜作了修改，现在，撤销修改就回到添加到暂存区后的状态.

- `git rm -f [filename]` 文件从版本库中删除
- `git checkout [filename]`  文件从版本库中恢复[git checkout其实是⽤用版本库⾥里的版本替换⼯工作区的版本]
- `git log`	显示当前分支的版本历史 
	-  **git log --pretty=oneline** 

	```
DragonLi-2:pythonRepo LFL$ git log --pretty=oneline
4d43c74252ea01a0ffac5cff71c5853259e208f2 (HEAD -> master) adjust diff fix #1
22c3340902e72394e7b2a5aab136cc76576f7ddb add coding
446a8635555c20af39fd1c1856734e72af6cbb70 python.py
	```
- **git reflog⽤用来记录你的每⼀次命令** 
	- 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本
	- `git reset --hard commit_id`   
- `git clone [url]`  URL
- `git remote add origin [url]`  本地仓库关联远程仓库
- `git push -u origin master` 
	-  加上了-u参数,Git不但会把本地的
master分⽀支内容推送的远程新的master分⽀支
	- 还会把本地的master分⽀支和远程的master 分⽀支关联起来在以后的推送或者拉取时就可以简化命令
- `git pull`   同步远程代码

## other operation 

### branch
- **git checkout -b [branchname]** 新建一个分支，并切换到该分支 
	- git checkout命令加上-b参数表⽰示创建并切换:`git branch [branchname]` && `git checkout [branchname]`
	- `git branch [branch-name]` 新建一个分支，但依然停留在当前分支
- `git checkout [branch-name]` 切换到指定分支，并更新工作区
- `git branch --track [branch] [remote-branch]` 新建一个分支，与指定的远程分支建立追踪关系,假如远程变化,本地通知 

- `git branch -a` 列出所有本地分支和远程分支  **当前分⽀支前⾯面会标⼀一个*号**
- `git branch -r` 列出所有远程分支
- `git branch --merged`               显示所有已合并到当前分支的分支
- `git branch --no-merged`     		   显示所有未合并到当前分支的分支

- `git branch -d [branch-name]` 删除分支
- `git push origin --delete [branch-name]` 删除远程分支

## 3.2 merge  fetch
- git merge [branch]
- git cherry-pick [commit]

- git fetch [remote] 下载远程仓库的所有变动 


## 3.3 conflict 

- 冲突的解决?

```
>>>>>>>>>>>>>>>>>>>
			HEAD
	653ghsjfkvdf4533fdsf			
>>>>>>>>>>>>>>>>>>>

```

## 3.4 tag

- git tag [tag]  新建一个tag在当前commit

- git tag -d [tag] 删除本地tag

- git push [remote] --tags 提交所有tag

- git checkout -b [branch] [tag] 新建一个分支，指向某个tag 

- git push origin :refs/tags/tagName 


## 3.5 Reset
-   git reset --hard HEAD^1

-   git reset —hard     commitID  回退指定版本

-  	git push --force   强制推送远程同步


## 3.6 补充 
- git diff origin/master..master    `比较远程分支master上有本地分支master上没有的`



	
# 4. supplement

### 4.1 Git stash 暂存操作 --->在开发的过程中，突然来了个紧急的BUG或是任务，但是当前的任务或BUG还没有修复好，代码也不能提交，怎么办呢？
- git stash list 查看当前暂存的列表

- git stash apply stash@{0}  取出某个暂存的代码


- git stash pop	 如果使用pop，取出最后一次存在的暂存，同时删除取出的暂存



## 4.2 gitconfig  配置

- Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。git config --list

- 第一次使用Git常用配置操作命令:

```
	1.配置个人信息
 	git config [--global] user.name "[name]"
  	git config [--global] user.email "[email address]"
  	
  	2. git status等命令自动着色
  	git config --global color.ui true                         

```


## 4.3 alias

-  git config alias.[new_name] "[old_name]"


-  git config alias.ci commit  给 commit 起一个别名叫 ci


## 4.4 终端如果觉得太繁琐,可以使用可视化工具.`SourceTree`等

