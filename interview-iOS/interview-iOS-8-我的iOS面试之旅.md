
## 我的iOS面试之旅(2018 -04)

> https://xcqromance.top/2018/04/01/2018-04-01/


### 常问的知识点

* 内存管理方面（ARC、MRC、autorelease、autoreleasepool）
* Runtime方面（消息发送，NSHipster 对象关联，NSHipster 方法交换等等）
* KVO内部实现原理（多家公司有问到）
* Runloop方面（深入理解RunLoop）
* HTTPS通信过程
* UITableView的优化手段方法（iOS 保持界面流畅的技巧）
* 多线程方面（GCD、NSOperation居多）（关于iOS多线程，你看我就够了）
* SDWebImage源码分析
* 事件传递以及响应链（史上最详细的iOS之事件的传递和响应机制）
* [图片的解压缩](http://blog.leichunfeng.com/blog/2017/02/20/talking-about-the-decompression-of-the-image-in-ios/)


#### 图片的解压缩概述	
>  在主线程的下一个 run loop 到来时，Core Animation 提交了这个隐式的 transaction ，这个过程可能会对图片进行 copy 操作，而受图片是否字节对齐等因素的影响，这个 copy 操作可能会涉及以下部分或全部步骤：

> 在将磁盘中的图片渲染到屏幕之前，必须先要得到图片的原始像素数据，才能执行后续的绘制操作，这就是为什么需要对图片解压缩的原因

* 	分配内存缓冲区用于管理文件 IO 和解压缩操作；
* 	将文件数据从磁盘读到内存中；
* 	将压缩的图片数据解码成未压缩的位图形式，这是一个非常耗时的 CPU 操作；
	*  解压缩后的图片大小与原始文件大小之间没有任何关系，而只与图片的像素有关 
* 	最后 Core Animation 使用未压缩的位图数据渲染 UIImageView 的图层。




### 需要了解的知识点

* APM方面（内存泄漏检测、crash监控，卡顿监控以及底层的实现原理等等）
* 组件化方（蘑菇街 App 的组件化之路、iOS应用架构谈 组件化方案、在现有工程中实施基于CTMediator的组件化方案、iOS 组件化方案探索、iOS 组件化–路由设计思路分析）
* 持续化集成（我们公司使用的是：Jenkins+fastlane）


## 链接

- [面试题系列目录](README.md)
- **上一份**: [2018-4月份iOS面试经历](interview-iOS-7-2018-4月份iOS面试经历.md)
- **下一份**: [一个渣硕iOS春招总结](interview-iOS-9-一个渣硕iOS春招总结.md)

