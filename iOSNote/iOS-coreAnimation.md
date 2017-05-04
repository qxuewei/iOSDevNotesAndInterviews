# iOS core animation -知识点记录

## 1. 图层树
				
### 为什么iOS要基于UIView和CALayer提供两个平行的层级关系呢？
- 为什么不用一个简单的层级来处理所有事情呢？原因在于要做职责分离，这样也能避免很多重复代码。在iOS和Mac OS两个平台上，事件和用户交互有很多地方的不同，基于多点触控的用户界面和基于鼠标键盘有着本质的区别，这就是为什么iOS有UIKit和UIView，但是Mac OS有AppKit和NSView的原因。他们功能上很相似，但是在实现上有着显著的区别


- 绘图，布局和动画，相比之下就是类似Mac笔记本和桌面系列一样应用于iPhone和iPad触屏的概念。把这种功能的逻辑分开并应用到独立的Core Animation框架，苹果就能够在iOS和Mac OS之间共享代码，使得对苹果自己的OS开发团队和第三方开发者去开发两个平台的应用更加便捷


### UIView没有暴露出来的CALayer的功能
- CALayer的好处在于，你能在使用所有CALayer底层特性的同时，也可以使用UIView的高级API（比如自动排版，布局和事件处理
- 阴影，圆角，带颜色的边框
- 3D变换
- 非矩形范围
- 透明遮罩
- 多级非线性动画

## 2.寄宿图

### contents属性(类型:`id`,但是必须是(CGImage/NSImage)才有内容,事实上是CGImageRef:Core Foundation类型)

```
UIImage *imageContent =[UIImage imageNamed:@"back-LFL"];
// Xcode 会自动红点,点击后加入(__bridge id _Nullable)
Layer.contents = (__bridge id _Nullable)(imageContent.CGImage);

```
### contentGravity:(CALayer与contentMode对应的属性叫做contentsGravity,type: NSString)

```
决定内容在图层的边界中怎么对齐的常量值

kCAGravityCenter
kCAGravityTop
kCAGravityBottom
kCAGravityLeft
kCAGravityRight
kCAGravityTopLeft
kCAGravityTopRight
kCAGravityBottomLeft
kCAGravityBottomRight
kCAGravityResize
kCAGravityResizeAspect
kCAGravityResizeAspectFill

eg: CA_EXTERN NSString * const kCAGravityResizeAspectFill

```
### contentsScale:定义了寄宿图的像素尺寸和视图大小的比例,默认1.0

```
//set the contentsScale to match image
layerView.layer.contentsScale = image.scale;
// 或者设置设备:
layer.contentsScale = [UIScreen mainScreen].scale;

```
### maskToBounds:决定是否显示超出边界的内容

### contentsRect:允许我们在图层边框里显示寄宿图的一个子域(使用单位坐标)

- 点 —— 在iOS和Mac OS中最常见的坐标体系。点就像是虚拟的像素，也被称作逻辑像素。在标准设备上，一个点就是一个像素，但是在Retina设备上，一个点等于2*2个像素。iOS用点作为屏幕的坐标测算体系就是为了在Retina设备和普通设备上能有一致的视觉效果。

- 像素 —— 物理像素坐标并不会用来屏幕布局，但是仍然与图片有相对关系。UIImage是一个屏幕分辨率解决方案，所以指定点来度量大小。但是一些底层的图片表示如CGImage就会使用像素，所以你要清楚在Retina设备和普通设备上，他们表现出来了不同的大小。

- 单位 —— 对于与图片大小或是图层边界相关的显示，单位坐标是一个方便的度量方式， 当大小改变的时候，也不需要再次调整。单位坐标在OpenGL这种纹理坐标系统中用得很多，Core Animation中也用到了单位坐标。

```
默认的contentsRect是{0, 0, 1, 1}，这意味着整个寄宿图默认都是可见的，如果我们指定一个小一点的矩形，图片就会被裁剪.

典型地，图片拼合后可以打包整合到一张大图上一次性载入。相比多次载入不同的图片，这样做能够带来很多方面的好处：内存使用，载入时间，渲染性能等等+

```
### contentsCenter:其实是一个CGRect,改变contentsCenter的值并不会影响到寄宿图的显示，除非这个图层的大小改变了，你才看得到效果。
- 可视化编程中对应:stretching  


### Custom Drawing
- -drawRect: 方法没有默认的实现，因为对UIView来说，寄宿图并不是必须的，它不在意那到底是单调的颜色还是有一个图片的实例。如果UIView检测到-drawRect: 方法被调用了，它就会为视图分配一个寄宿图，这个寄宿图的像素尺寸等于视图大小乘以 contentsScale的值


- 如果你不需要寄宿图，那就不要创建这个方法了，这会造成CPU资源和内存的浪费，这也是为什么苹果建议：如果没有自定义绘制的任务就不要在子类中写一个空的-drawRect:方法

- -drawRect:方法里面的代码利用Core Graphics去绘制一个寄宿图，然后内容就会被缓存起来直到它需要被更新（通常是因为开发者调用了-setNeedsDisplay方法，尽管影响到表现效果的属性值被更改时，一些视图类型会被自动重绘，如bounds属性）。虽然-drawRect:方法是一个UIView方法，事实上都是底层的CALayer安排了重绘工作和保存了因此产生的图片。

- CALayer有一个可选的delegate属性，实现了CALayerDelegate协议(均为可选)，当CALayer需要一个内容特定的信息时，就会从协议中请求.
	- 我们在blueLayer上显式地调用了-display。不同于UIView，当图层显示在屏幕上时，CALayer不会自动重绘它的内容。它把重绘的决定权交给了开发者。
	- 尽管我们没有用masksToBounds属性，绘制的那个圆仍然沿边界被裁剪了。这是因为当你使用CALayerDelegate绘制寄宿图的时候，并没有对超出边界外的内容提供绘制支持。 

```
当需要被重绘时，CALayer会请求它的代理给他一个寄宿图来显示。它通过调用下面这个方法做到的:
-(void)displayLayer:(CALayerCALayer *)layer;

趁着这个机会，如果代理想直接设置contents属性的话，它就可以这么做，不然没有别的方法可以调用了。如果代理不实现-displayLayer:方法，CALayer就会转而尝试调用下面这个方法：
-(void)drawLayer:(CALayer *)layer inContext:(CGContextRef)ctx;

```

## 3.图层几何学

### 布局:UIView有三个比较重要的布局属性：frame，bounds和center，CALayer对应地叫做frame，bounds和position

-  对于视图或者图层来说，frame并不是一个非常清晰的属性，它其实是一个虚拟属性，是根据bounds，position和transform计算而来，所以当其中任何一个值发生改变，frame都会变化。相反，改变frame的值同样会影响到他们当中的值

### 锚点:anchorPoint位于图层的中点，所以图层的将会以这个点为中心放置

- 图层左上角是{0, 0}，右下角是{1, 1}，因此默认坐标是{0.5, 0.5}。

```
// 旋转
self.imageView.layer.anchorPoint = CGPointMake(0.5f, 0.9f); 

```
###坐标系

- UIView严格的二维坐标系不同，CALayer存在于一个三维空间当中(zPosition和anchorPointZ)
- egCode:`    self.greenView.layer.zPosition = 1.0f;`

### Hit Testing
- containsPoint : `if ([self.layerView.layer containsPoint:point])`
- hitTest	:接受一个CGPoint类型参数,返回本身

```
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    //get touch position
    CGPoint point = [[touches anyObject] locationInView:self.view];
    //get touched layer
    CALayer *layer = [self.layerView.layer hitTest:point];
    //get layer using hitTest
    if (layer == self.blueLayer) {
  }  
}

```
### 自动布局
- (void)layoutSublayersOfLayer:(CALayer *)layer;
-  当图层的bounds发生改变，或者图层的-setNeedsLayout方法被调用的时候，上面这个函数将会被执行,这使得你可以手动地重新摆放或者重新调整子图层的大小，但是不能像UIView的autoresizingMask和constraints属性做到自适应屏幕旋转

##4. 视觉效果

- 圆角:`self.layerView2.layer.cornerRadius = 20.0f;`
- 边框:`self.layerView2.layer.borderWidth = 5.0f;`
- 阴影:












