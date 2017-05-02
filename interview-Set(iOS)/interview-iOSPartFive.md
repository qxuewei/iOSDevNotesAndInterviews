# interview-iOS PartFive

# Question list

-  界面多个网络请求,如何处理刷新的?
-  如果tableView界面网络请求有缓存数据逻辑?
-  init方法私有化?
-  线程中栈与堆是公有的还是私有的 ?


## 界面多个网络请求,如何处理刷新的?

- 使用调度组:**问题 : 为多个请求均加载完成，但界面已在未得到数据前提前刷新导致界面空白**
	- dispatch_semaphore信号量为基于计数器的一种多线程同步机制。用于解决在多个线程访问共有资源时候，会因为多线程的特性而引发数据出错的问题。

	- semaphore计数大于等于1，计数-1，返回，程序继续运行。如果计数为0，则等待。

	- dispatch_semaphore_signal(semaphore)为计数+1操作。
	
	-  dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER)为设置等待时间，这里设置的等待时间是一直等待


```
1. 调度组
dispatch_group_t group = dispatch_group_create();
    
    dispatch_group_async(group, dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        NSLog(@"netWorking_Frist");
        
    });
    
    dispatch_group_async(group, dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        NSLog(@"netWorking_Second");
    });
    dispatch_group_async(group,dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        NSLog(@"netWorking_Three");
    });
    
    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
        NSLog(@"complete");
    });
    
        
2. 信号量

- (void)netWorkingimplementation{ // 基于自身项目的网络请求
//    1. 发起请求
    dispatch_semaphore_t  semaphore = dispatch_semaphore_create(0);
//    2. 成功/失败回调标记
    dispatch_semaphore_signal(semaphore);
//    3. 计数为0 ,则一直等待
    dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);
}
    
```


## 如果tableView界面网络请求有缓存数据逻辑?

- 首次进入一个界面tableview的数据应该先读取缓存，主要防止没有网络或者网络不好的情况下用户等待数据时间过长或者没有数据用户体验不好，主要用于某些tableview请求的数据量很大，或者数据短时间内变化不是很快的地方。
- 用户下拉刷新表示重新请求数据，应该强制更新数据，不论本地是否有缓存,但是这部分数据应该存入数据库，保证退出后下次进入界面的时候能够读取最后更新的缓存。
- 上拉加载更多的时候不读缓存，因为如果客户端自己在处理这部分逻辑比较复杂，不是说实现起来复杂，因为对于本地存储来说调用的是统一的一个方法，主要是因为如果用户下拉刷新需要强制更新数据，不论本地有无缓存，都要从服务器请求最新数据。那么问题来了，如果我把用户上拉加载更多时候的数据也存入本地，假设用户上拉加载了5页数据，然后又强制下拉刷新了一下，这个时候tableview显示的是最新的第一页数据，如果接着上拉加载更多我应该是读取缓存还是直接发起网络请求呢。毫无疑问应该直接发起请求，因为下拉强制刷新已经导致了第一页为最新数据，如果第2-5页数据缓存没有过期并且服务器数据确实变化了的话，客户端将得不到新的数据，所以干脆仅仅第一次进入界面tableview请求第一页数据的时候要读缓存。
- 根本不用缓存的地方主要是某些数据变化非常快，或者发起的网络请求是一次性的操作，比如收藏某个题目等等。


## init方法私有化
	
```
1 .h 
- (instancetype)init __attribute__((unavailable("Disabled. Use +sharedInstance instead")));
- (instancetype)init NS_UNAVAILABLE;

2.m
//3. 内部不响应 不建议!
- (instancetype)init {
   // 抛出不识别,没有说明真的原因
    [super doesNotRecognizeSelector:_cmd];
    return nil;
}

// 通过断言
- (instancetype)init1{
    NSAssert(false,@"unavailable, use sharedInstance instead");
    return nil;
}

// 通过异常
- (instancetype)init2{
    [NSException raise:NSGenericException format:@"Disabled. Use +[%@ %@] instead",
     NSStringFromClass([self class]),
     NSStringFromSelector(@selector(sharedInstance))];
    
    return nil;
}

```	 
## 线程中栈与堆是公有的还是私有的 ?
- 栈私有, 堆公有  






