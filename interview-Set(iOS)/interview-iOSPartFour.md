
# [GitHub HomePage](https://github.com/DevDragonLi)

# interview-iOS PartFour 

## 用户需要上传和下载一个重要的资料文件，应该如何判断用户本次是否上传成功和下载成功了?
- 用MD5验证文件的完整性！(仅仅通过代码来判断当前次的请求发送结束或者收到数据结束不可以的)
- 当客户端上传一个文件的时候，在请求body里面添加该文件的MD5值来告诉服务器，服务器接受文件完毕以后通过校验收到的文件的MD5值与请求body里面的MD5值来最终确定本次上传是否成功
- 当客户端下载一个文件的时候，在响应头里面收到了服务器附带的该文件的MD5值，文件下载结束以后，通过获取下载后文件的MD5值与本次请求服务器返回的响应头中的MD5值做一个比较，来最终判断本次下载是否成功
- MD5，是一个将任意长度的数据字符串转化成短的固定长度的值的单向操作。任意两个字符串不应有相同的散列值
- MD5校验可以应用在多个领域，比如说机密资料的检验，下载文件的检验，明文密码的加密等。MD5校验原理举例：如客户往我们数据中心同步一个文件，该文件使用MD5校验，那么客户在发送文件的同时会再发一个存有校验码的文件，我们拿到该文件后做MD5运算，得到的计算结果与客户发送的校验码相比较，如果一致则认为客户发送的文件没有出错，否则认为文件出错需要重新发送。


## ReactiveCocoa(RAC)如何防止UIButton短时间内多次重复点击，大概思路? 
- 建立一个`flag`或者使用 `filter`



- 事件完成 flag重置，否则一直skip或者filter (某个RAC群友抛砖引玉)



## 倒计时如何实现 ？
- NSTimer 
- GCD 
- RAC 


```
-(void)startTime{
    __block int timeout=30; //倒计时时间
    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    dispatch_source_t _timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0,queue);
    dispatch_source_set_timer(_timer,dispatch_walltime(NULL, 0),1.0*NSEC_PER_SEC, 0); //每秒执行
    dispatch_source_set_event_handler(_timer, ^{
        if(timeout<=0){ //倒计时结束，关闭
            dispatch_source_cancel(_timer);
            dispatch_async(dispatch_get_main_queue(), ^{
                //设置界面的按钮显示 根据自己需求设置
                [l_timeButton setTitle:@"发送验证码" forState:UIControlStateNormal];
                l_timeButton.userInteractionEnabled = YES;
            });
        }else{
            //            int minutes = timeout / 60;
            int seconds = timeout % 60;
            NSString *strTime = [NSString stringWithFormat:@"%.2d", seconds];
            dispatch_async(dispatch_get_main_queue(), ^{
                //设置界面的按钮显示 根据自己需求设置
                NSLog(@"____%@",strTime);
                [l_timeButton setTitle:[NSString stringWithFormat:@"%@秒后重新发送",strTime] forState:UIControlStateNormal];
                l_timeButton.userInteractionEnabled = NO;
                
            });
            timeout--;
            
        }
    });
    dispatch_resume(_timer);
    
}

 [[RACSignal interval:1 onScheduler:[RACScheduler mainThreadScheduler]]subscribeNext:^(NSDate * date) {
        static NSInter = 60; 
        60 --;
       //  处理显示逻辑
    }];
    
```


## 项目中你是怎么处理网络速度慢、中断抖动等网络请求中的问题?
- 等待补充


## 项目中的图片上传功能如何实现，为什么使用队列上传，为什么不用异步上传
- 等待补充



## isa指针的作用
- 对象的isa指向类，类的isa指向元类（meta class），元类isa指向元类的根类。isa帮助一个对象找到它的方法
- 是一个Class 类型的指针. 每个实例对象有个isa的指针,他指向对象的类，而Class里也有个isa的指针, 指向meteClass(元类)。元类保存了类方法的列表。当类方法被调用时，先会从本身查找类方法的实现，如果没有，元类会向他父类查找该方法。同时注意的是：元类（meteClass）也是类，它也是对象。元类也有isa指针,它的isa指针最终指向的是一个根元类(root meteClass).根元类的isa指针指向本身，这样形成了一个封闭的内循环。










