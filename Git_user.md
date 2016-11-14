#			Git 相关

## 1. Git branch

### 查看所有分支
### git branch -a
### git branch -av

### 切换本地分支
### git checkout master-dev

### 切换并创建本地分支
### git checkout -b master-dev

### 创建本地分支 并指向远程某个分支
### git checkout -b master-dev remotes/origin/master-dev

## 2.  git stash 暂存操作

### git stash
### 使用场景：
### 在开发的过程中，突然来了个紧急的BUG或是任务，但是当前的任务或BUG还没有修复好，代码也不能提交，怎么办呢？

### 这时候git stash 就可以发挥作用了，暂存到本地仓库。 
### 查看当前暂存的列表
### git stash list
### 取出某个暂存的代码
### git stash apply stash@{0}
### 使用apply取出暂存的代码，暂存数据还存在
### 如果使用pop，取出最后一次存在的暂存，同时删除取出的暂存
### git stash pop