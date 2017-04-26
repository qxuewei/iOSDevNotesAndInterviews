#  Git终端命令的使用
				
						
# 一 : why Git?	

- `SVN`的**安全控制**和**权限管理**更好。`git`下，如果一个人clone以后，所有代码和历史都泄漏了。而 svn 有细致的**按照目录级的权限控制** (为什么还有那么多人用SVN?) 

- [蒋鑫：为什么 Git 比 SVN 好](http://blog.jobbole.com/20069/)

- [为什么要用GIT而不是SVN](http://www.cnblogs.com/wi100sh/p/4191080.html)
				
#二:	Git初步体验

- git init [project-name] 新建一个目录，将其初始化为Git代码库 
- git add [name] 添加当前目录的所有文件到暂存区 (和svn的不同)

- git status 	显示有变更的文件 


- git commit -m "message" 提交日志

- git commit --amend -m [message]如果代码没有任何新变化，则用来改写上一次commit的提交信息

- git commit -am 'message'   `将add和commit合为一步` 
- git log	显示当前分支的版本历史 `gitref log`
- git push  


- git pull 


- git clone  

 
#三:	Git使用入门
## 3.1branch
- git branch -a 列出所有本地分支和远程分支


- git branch -r 列出所有远程分支
- git branch --merged               显示所有已合并到当前分支的分支
- git branch --no-merged     		   显示所有未合并到当前分支的分支

- git branch [branch-name] 新建一个分支，但依然停留在当前分支


- git checkout -b [branch] 新建一个分支，并切换到该分支

- git branch --track [branch] [remote-branch] 新建一个分支，与指定的远程分支建立追踪关系,假如远程变化,本地通知 

- git checkout [branch-name] 切换到指定分支，并更新工作区

- git branch -d [branch-name] 删除分支

- git push origin --delete [branch-name] 删除远程分支

-
 

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

## 4.1 Git stash 暂存操作 --->在开发的过程中，突然来了个紧急的BUG或是任务，但是当前的任务或BUG还没有修复好，代码也不能提交，怎么办呢？
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

