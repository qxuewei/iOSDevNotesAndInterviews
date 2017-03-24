
# [GitHub HomePage](https://github.com/DevDragonLi)

# 官网的安装教程,目前是0.42 
##1. 必需的软件

```
sudo chown -R `whoami` /usr/local

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```

`Node`

```
brew install node

npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global

```
`Yarn、React Native的命令行工具`

```
npm install -g yarn react-native-cli

yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global

```

##2. 推荐安装的工具
```
brew install watchman
brew install flow


```
### 3.测试安装
```
react-native init ProjectDemo
cd ProjectDemo
react-native run-ios

```




# 二.如果你有好的资源,也可以提交或者联系我. 
 <dragonli_52171@163.com>   