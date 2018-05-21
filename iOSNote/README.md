## note list  

### <a name="iOSDevNote"></a> iOSDevNote

| CATEGORY | FILE |  
|:----|:----|
|iOSDevNote|[**iOS架构相关**](./iOS_architecture.pdf)<br>[**掘金客户端体积瘦身**](./appThin/readme.md)<br>[**iOSDevCodeRepo**](https://github.com/DevDragonLi/iOSDevDemo)<br>[Core Animation框架结构及性能调优11张大图详解](https://github.com/DevDragonLi/Core-AnimationPerformanceOptimization)<br>[iOS_StaticLibrary](./iOS_StaticLibrary/readme.md)<br>[iOS经典Crash分析与总结](./Crash/README.md)<br>[iOS核心动画高级技巧阅读笔记(**性能调优**,**高效绘制**,**图像IO**,**图层性能**)](./Article/iOS-coreAnimationNote.md)<br>[单元测试概述](./Article/UnitTesting.md)|
|CocoaPods 相关 |[CocoaPods提交开源的框架之流程](./CocoaPods/cocoapods-podspec.md)<br> [CocoaPods管理库的使用技巧](./CocoaPods/CocoaPods管理库的使用技巧.md)<br>[CocoaPods提交私有的框架之流程](./CocoaPods/Pod&&spec.md) <br>[CocoaPods私有库参考Demo](https://github.com/DevDragonLi/iOSDevDemo/tree/master/1-DevDemo/PodPrivate_demo )|
|iOS技能图谱|[iOS技能图谱-byStuQ](./Article/map-MobileDev-iOSDev.md)<br>[JSPatch作者博客中的技能树](../images/iOSDev-bang.png)|


### 关于`TableView`的[tableView: heightForRowAtIndexPath:]和[tableView: cellForRowAtIndexPath: ]历史版本调用顺序的变迁

- iOS7及之前: 
	-  **先依次调一遍heightForRow方法再依次调一遍cellForRow方法**，在调cellForRow方法的时候并不会再调一次对应的heightForRow方法。
	-  如果我们实现了：[-tableView: estimatedHeightForRowAtIndexPath:]给了系统估计高度，那么上述两个方法的执行顺序就会颠倒。并且给定估计高度对于TableView的性能方面也提示不少。

- iOS8
	- 先依次调heightForRow**（如果行数超过屏幕依次调用两次，如果行数很少，没有超过屏幕，只依次调用一次）**
	- 之后每调一次cellForRow的时候又调一次对应的heightForRow方法。

- iOS9和iOS10:
	- `heightForRow方法会先调用三次`，
	- 然后每调用一次cellForRow的时候再调用一次对应的heightForRow。

- iOS11:
	-  先row = 0调用一次 cellForRow，然后一次heightForRow.然后再是row =1 ,依次类推