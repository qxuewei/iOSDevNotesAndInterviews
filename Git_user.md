#			Git 相关

## 				 Git branch

### 1.查看所有分支
### git branch -a
### git branch -av

### 2.切换本地分支
### git checkout master-dev

### 3.切换并创建本地分支
### git checkout -b master-dev

### 4.创建本地分支 并指向远程某个分支
### git checkout -b master-dev remotes/origin/master-dev

### 5.要删除服务器远端的分支，则执行如下所示的命令：
### git push origin –delete 分支名
### 6.如果是要删除本地已经合并了的分支，则执行：
### git branch –d 分支名
### 7.删除本地未合并的分支：
###  git branch –D 分支名


## Git stash 暂存操作

### 1.git stash
### 使用场景：
### 在开发的过程中，突然来了个紧急的BUG或是任务，但是当前的任务或BUG还没有修复好，代码也不能提交，怎么办呢？

### 这时候git stash 就可以发挥作用了，暂存到本地仓库。 
### 查看当前暂存的列表
### 2.git stash list
### 取出某个暂存的代码
### 3. git stash apply stash@{0}
### 使用apply取出暂存的代码，暂存数据还存在
### 如果使用pop，取出最后一次存在的暂存，同时删除取出的暂存
### 4.git stash pop