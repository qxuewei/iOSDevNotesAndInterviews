# CocoaPods提交私有的框架之流程摘要

## base pod 

- **cocoapods**: **/Users/`LFL`/.cocoapods/repos**

- **cache**: **/Users/`LFL`/Library/Caches/CocoaPods/** 包含如下
	- pods :缓存已经安装的第三方
	- search_index.json 

 
- **pic资源文件**

```
 图片bundle 资源 
 
[[NSbundle bundleForClass :self] pathForResource:"" inDirectory:"当前pod库名.bundle"]

```
- **xib 处理** 
 - 先编译为 `nib`
 - 读取涉及图片,需要处理对应的`@2`,`@3`
 - 
 
 
 
- **子库分离 参考 "AFN"**
	
`s.subspec 'LFLSegumentTool' do |b| 
b.source_files => 'LFLTest/LFLSegumentTool/*'
子库依赖单独处理
b.dependecy = 'GitHubSegumentTool'
end`

### example 

- 私有的远程索引  pod repo add LFLcocoapods URL  部署到`coding` ,`oschina`等

- pod repo remove LFLcocoapods  

- pod repo update LFLcocoapods

- pod spec lint 验证自有库 

- pod repo push LFLTest LFLTest.podspec


