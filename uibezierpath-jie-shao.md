### UIBezierPath

> 看了前面，大家是不是都想来看看UIBezierPath详解呢，然后开始展示真正的装逼技术撒，其实，看完你就会发现UIBezierPath也就那么回事（先泼一盆冷水）

#### 创建UIBezierPath的类方法介绍

* 初始化

```
+ (instancetype) bezierPath;
```

* 初始化矩形

```
/**
  * 该方法将会创建一个闭合路径, 起始点是 rect 参数的的 origin, 并且按照顺时针方向添加直线, 最终形成矩形
  * @param rect:   矩形路径的 Frame
  */
+ (instancetype)bezierPathWithRect:(CGRect)rect;
```

* 椭圆形

```
/**
  * 该方法将会创建一个闭合路径,  该方法会通过顺时针的绘制贝塞尔曲线, 绘制出一个近似椭圆的形状. 如果 rect 参数指定了一个矩形, 那么该 UIBezierPath 对象将会描述一个圆形.
  * @param rect:   矩形路径的 Frame
  */
+ (instancetype)bezierPathWithOvalInRect:(CGRect)rect;
```

* 圆角矩形

```
/**
  * 该方法将会创建一个闭合路径,  该方法会顺时针方向连续绘制直线和曲线.  当 rect 为正方形时且 cornerRadius 等于边长一半时, 则该方法会描述一个圆形路径.
  * @param rect:   矩形路径的 Frame
  * @param cornerRadius:   矩形的圆角半径
  */
+ (instancetype) bezierPathWithRoundedRect:(CGRect)rect 
                              cornerRadius:(CGFloat)cornerRadius;
```

* 特定的圆角矩形

```
/**
  * 该方法将会创建一个闭合路径,  该方法会顺时针方向连续绘制直线和曲线.  
  * @param rect:   矩形路径的 Frame
  * @param corners:   UIRectCorner 枚举类型, 指定矩形的个角变为圆角
  * @param cornerRadii:   矩形的圆角半径
  */
+ (instancetype) bezierPathWithRoundedRect:(CGRect)rect 
                         byRoundingCorners:(UIRectCorner)corners
                               cornerRadii:(CGSize)cornerRadii;
```

* 弧度

```
/**
  * 该方法会创建出一个开放路径, 创建出来的圆弧是圆的一部分. 在默认的坐标系统中, 开始角度 和 结束角度 都是基于单位圆的(看下面这张图). 调用这个方法之后, currentPoint 将会设置为圆弧的结束点.
  * 举例来说: 指定其实角度为0, 指定结束角度为π, 设置 clockwise 属性为 YES, 将会绘制出圆的下半部分.
  * 然而当我们不修改起始角度 和 结束角度, 我们仅仅将 clockwise 角度设置为 NO, 则会绘制出来一个圆的上半部分.
  * @param center:   圆心
  * @param radius: 半径
  * @param startAngle:   起始角度
  * @param endAngle:   结束角度
  * @param clockwise:   是否顺时针绘制 Yes顺，NO逆时针
  */
+ (instancetype) bezierPathWithArcCenter:(CGPoint)center 
                                  radius:(CGFloat)radius 
                              startAngle:(CGFloat)startAngle 
                                endAngle:(CGFloat)endAngle 
                               clockwise:(BOOL)clockwise;
```

![](/assets/2452150-5bf0e532c2a0f6d9.jpg)

#### 根据具体路径返回形状

```
+ (instancetype) bezierPathWithCGPath:(CGPathRef)CGPath;
```

* 修改绘制方向

```
/**
  * 通过该方法反转一条路径, 并不会修改该路径的样子. 它仅仅是修改了绘制的方向
  * @return: 返回一个新的 UIBezierPath 对象, 形状和原来路径的形状一样,
  *          但是绘制的方向相反.
  */
- (UIBezierPath *) bezierPathByReversingPath;
```

* 开始点（将currentPoint移动到指定的点）

```
// 将开始描绘的点移动到某一点
- (void)moveToPoint:(CGPoint)point;
```

* 添加直线

```
/**
  * 该方法将会从 currentPoint 到 指定点 链接一条直线. 
  * Note: 在追加完这条直线后, 该方法将会更新 currentPoint 为 指定点
  *       调用该方法之前, 你必须先设置 currentPoint. 如果当前绘制路径
  *       为空, 并且未设置 currentPoint, 那么调用该方法将不会产生任何
  *       效果.
  * @param point:   绘制直线的终点坐标, 当前坐标系统中的某一点
  */
- (void)addLineToPoint:(CGPoint)point;
```

* 添加弧线

/\*\*

\* 该方法将会从 currentPoint 添加一条指定的圆弧.

\* 该方法的介绍和构造方法中的一样. 请前往上文查看

\* @param center: 圆心

\* @param radius: 半径

\* @param startAngle: 起始角度

\* @param endAngle: 结束角度

\* @param clockwise: 是否顺时针绘制

\*/

* \(void\)addArcWithCenter:\(CGPoint\)center

  ```
              radius:\(CGFloat\)radius 

          startAngle:\(CGFloat\)startAngle 

            endAngle:\(CGFloat\)endAngle 

           clockwise:\(BOOL\)clockwise NS\_AVAILABLE\_IOS\(4\_0\);
  ```

* 添加一条三次贝塞尔曲线

```
/**
  * 该方法将会从 currentPoint 到 指定的 endPoint 追加一条三次贝塞尔曲线.
  * 三次贝塞尔曲线的弯曲由两个控制点来控制. 如下图所示
  * Note: 调用该方法前, 你必须先设置 currentPoint, 如果路径为空, 
  *       并且尚未设置 currentPoint, 调用该方法则不会产生任何效果. 
  *       当添加完贝塞尔曲线后, 该方法将会自动更新 currentPoint 为
  *       指定的结束点
  * @param endPoint: 终点
  * @param controlPoint1: 控制点1
  * @param controlPoint2: 控制点2
  */
- (void)addCurveToPoint:(CGPoint)endPoint 
          controlPoint1:(CGPoint)controlPoint1 
          controlPoint2:(CGPoint)controlPoint2;
```

![](/assets/2452150-fabfa795e061d718.jpg)

* 添加一条二次贝塞尔曲线

```
/**
  * 该方法将会从 currentPoint 到 指定的 endPoint 追加一条二次贝塞尔曲线.
  * currentPoint、endPoint、controlPoint 三者的关系最终定义了二次贝塞尔曲线的形状.
  * 二次贝塞尔曲线的弯曲由一个控制点来控制. 如下图所示
  * Note: 调用该方法前, 你必须先设置 currentPoint, 如果路径为空, 
  *       并且尚未设置 currentPoint, 调用该方法则不会产生任何效果. 
  *       当添加完贝塞尔曲线后, 该方法将会自动更新 currentPoint 为
  *       指定的结束点
  * @param endPoint: 终点
  * @param controlPoint: 控制点
  */
- (void)addQuadCurveToPoint:(CGPoint)endPoint 
               controlPoint:(CGPoint)controlPoint;
```

![](/assets/2452150-3d4952efead3a84c.jpg)

* 关闭子路径（连接一条线到起点）

```
/**
  * 该方法将会从 currentPoint 到子路经的起点 绘制一条直线, 
  * 以此来关闭当前的自路径. 紧接着该方法将会更新 currentPoint
  * 为 刚添加的这条直线的终点, 也就是当前子路经的起点.
  */
- (void)closePath;
```

* 删除所有路径（点）

```
- (void)removeAllPoints;
```

* 将指定的UIBezierPath中的内容添加到当前UIBezierPath中

```
/**
  * 该方法将会在当前 UIBezierPath 对象的路径中追加
  * 指定的 UIBezierPath 对象中的内容. 
  */
- (void)appendPath:(UIBezierPath *)bezierPath;
```

#### 绘图属性

* 线宽

  /\*\*

  * 线宽属性定义了 `UIBezierPath` 对象中绘制的曲线规格. 默认为: 1.0
    \*/
    @property\(nonatomic\) CGFloat lineWidth;

* 曲线终点样式

```
/**
  * 该属性应用于曲线的终点和起点. 该属性在一个闭合子路经中是无效果的. 默认为: kCGLineCapButt
  */
@property(nonatomic) CGLineCap lineCapStyle;


// CGPath.h
/* Line cap styles. */
typedef CF_ENUM(int32_t, CGLineCap) {
    kCGLineCapButt,
    kCGLineCapRound,
    kCGLineCapSquare
};
```

![](/assets/2452150-cd1b858ce7df9dd6.png.jpeg)

* 曲线连接样式

```
/**
  * 默认为: kCGLineJoinMiter.
  */
@property(nonatomic) CGLineJoin lineJoinStyle;


// CGPath.h
/* Line join styles. */
typedef CF_ENUM(int32_t, CGLineJoin) {
    kCGLineJoinMiter,
    kCGLineJoinRound,
    kCGLineJoinBevel
};
```

![](/assets/2452150-71112236fff490b8.png.jpeg)

* 内角和外角的距离

```
/**
  * 两条线交汇处内角和外角之间的最大距离, 只有当连接点样式为 kCGLineJoinMiter
  * 时才会生效，最大限制为10
  * 我们都知道, 两条直线相交时, 夹角越小, 斜接长度就越大.
  * 该属性就是用来控制最大斜接长度的.
  * 当我们设置了该属性, 如果斜接长度超过我们设置的范围, 
  * 则连接处将会以 kCGLineJoinBevel 连接类型进行显示.
  */
@property(nonatomic) CGFloat miterLimit;
```

![](/assets/2452150-d6647c67c61e87c6.png.jpeg)

* 渲染精度

```
/**
  * 该属性用来确定渲染曲线路径的精确度.
  * 该属性的值用来测量真实曲线的点和渲染曲线的点的最大允许距离.
  * 值越小, 渲染精度越高, 会产生相对更平滑的曲线, 但是需要花费更
  * 多的计算时间. 值越大导致则会降低渲染精度, 这会使得渲染的更迅
  * 速. flatness 的默认值为 0.6.
  * Note: 大多数情况下, 我们都不需要修改这个属性的值. 然而当我们
  *       希望以最小的消耗去绘制一个临时的曲线时, 我们也许会临时增
  *       大这个值, 来获得更快的渲染速度.
  */

@property(nonatomic) CGFloat flatness;
```

* 虚线

```
/**
  * @param pattern: 该属性是一个 C 语言的数组, 其中每一个元素都是 CGFloat
  *                 数组中的元素代表着线段每一部分的长度, 第一个元素代表线段的第一条线,
  *                 第二个元素代表线段中的第一个间隙. 这个数组中的值是轮流的. 来解释一下
  *                 什么叫轮流的. 
  *                 举个例子: 声明一个数组 CGFloat dash[] = @{3.0, 1.0}; 
  *                 这意味着绘制的虚线的第一部分长度为3.0, 第一个间隙长度为1.0, 虚线的
  *                 第二部分长度为3.0, 第二个间隙长度为1.0. 以此类推.

  * @param count: 这个参数是 pattern 数组的个数
  * @param phase: 这个参数代表着, 虚线从哪里开始绘制.
  *                 举个例子: 这是 phase 为 6. pattern[] = @{5, 2, 3, 2}; 那么虚线将会
  *                 第一个间隙的中间部分开始绘制, 如果不是很明白就请继续往下看,
  *                 下文实战部分会对虚线进行讲解.
  */
- (void)setLineDash:(const CGFloat *)pattern
              count:(NSInteger)count
              phase:(CGFloat)phase;
```

* 重新获取虚线样式

```
/**
  * 该方法可以重新获取之前设置过的虚线样式.
  *  Note:  pattern 这个参数的容量必须大于该方法返回数组的容量.
  *         如果无法确定数组的容量, 那么可以调用两次该方法, 第一次
  *         调用该方法的时候, 传入 count 参数, 然后在用 count 参数
  *         来申请 pattern 数组的内存空间. 然后再第二次正常的调用该方法
  */
- (void)getLineDash:(CGFloat *)pattern 
              count:(NSInteger *)count
              phase:(CGFloat *)phase;
```

#### 绘制路径

* 填充

```
/**
  * 该方法当前的填充颜色 和 绘图属性对路径的封闭区域进行填充.
  * 如果当前路径是一条开放路径, 该方法将会隐式的将路径进行关闭后进行填充
  * 该方法在进行填充操作之前, 会自动保存当前绘图的状态, 所以我们不需要
  * 自己手动的去保存绘图状态了. 
  */
- (void)fill;
```

* 描边

```
- (void)stroke;
```

* 混合

```
/**
  * 该方法当前的填充颜色 和 绘图属性 (外加指定的混合模式 和 透明度) 
  * 对路径的封闭区域进行填充. 如果当前路径是一条开放路径, 该方法将
  * 会隐式的将路径进行关闭后进行填充
  * 该方法在进行填充操作之前, 会自动保存当前绘图的状态, 所以我们不需要
  * 自己手动的去保存绘图状态了. 
  *
  * @param blendMode: 混合模式决定了如何和已经存在的被渲染过的内容进行合成
  * @param alpha: 填充路径时的透明度
  */
- (void)fillWithBlendMode:(CGBlendMode)blendMode 
                    alpha:(CGFloat)alpha;

- (void)strokeWithBlendMode:(CGBlendMode)blendMode
                      alpha:(CGFloat)alpha;
```

#### 路径剪切

```
/**
  *  该方法将会修改当前绘图上下文的可视区域.
  *  当调用这个方法之后, 会导致接下来所有的渲染
  *  操作, 只会在剪切下来的区域内进行, 区域外的
  *  内容将不会被渲染.
  *  如果你希望执行接下来的绘图时, 删除剪切区域,
  *  那么你必须在调用该方法前, 先使用 CGContextSaveGState 方法
  *  保存当前的绘图状态, 当你不再需要这个剪切区域
  *  的时候, 你只需要使用 CGContextRestoreGState 方法
  *  来恢复之前保存的绘图状态就可以了.
  */
- (void)addClip;
```

#### 一些判断

* 是否包含某个点

```
/**
  *  该方法返回一个布尔值, 当曲线的覆盖区域包含
  * 指定的点(内部点)， 则返回 YES, 否则返回 NO. 
  * Note: 如果当前的路径是一个开放的路径, 那么
  *       就算指定点在路径覆盖范围内, 该方法仍然会
  *       返回 NO, 所以如果你想判断一个点是否在一个
  *       开放路径的范围内时, 你需要先Copy一份路径,
  *       并调用 -(void)closePath; 将路径封闭, 然后
  *       再调用此方法来判断指定点是否是内部点.
  * @param point: 指定点.
  */
- (BOOL) containsPoint:(CGPoint)point;
```

* 路径是否为空

```
/**
  * 检测当前路径是否绘制过直线或曲线.
  * Note: 记住, 就算你仅仅调用了 moveToPoint 方法
  *       那么当前路径也被看做不为空.
  */
@property (readonly, getter=isEmpty) BOOL empty;
```

* 路径覆盖的矩形区域

```
/**
  * 该属性描述的是一个能够完全包含路径中所有点
  *  的一个最小的矩形区域. 该区域包含二次贝塞尔
  *  曲线和三次贝塞尔曲线的控制点.
  */
```

#### 放射变换

```
/**
  * 该方法将会直接对路径中的所有点进行指定的放射
  * 变换操作. 
  */
- (void)applyTransform:(CGAffineTransform)transform;
```

#### 画个虚线

```
    UIBezierPath *path =[UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(10, 50)];
    [path addLineToPoint:CGPointMake(300, 50)];
    path.lineWidth = 2;
    [[UIColor redColor] set];
   
    
    UIBezierPath *path1 =[UIBezierPath bezierPath];
    [path1 moveToPoint:CGPointMake(10, 100)];
    [path1 addLineToPoint:CGPointMake(300, 100)];
    path1.lineWidth = 2;
   
    UIBezierPath *path2 =[UIBezierPath bezierPath];
    [path2 moveToPoint:CGPointMake(10, 150)];
    [path2 addLineToPoint:CGPointMake(300, 150)];
    path2.lineWidth = 2;
   
    
    CGFloat dash[] = {20.0,10.0};
    CGFloat dash1[] = {20.0,10.0,40.0,20.0};
    CGFloat dash2[] = {30.0,10.0};
    [path setLineDash:dash count:2 phase:0];
    // 注意这里如果是三那个只会用到20，10，40，后面的20用不到
    [path1 setLineDash:dash1 count:3 phase:0];
    [path2 setLineDash:dash2 count:2 phase:2];
    
    
     [path stroke];
     [path1 stroke];
     [path2 stroke];
```

![](/assets/Snip20180426_20.png)

