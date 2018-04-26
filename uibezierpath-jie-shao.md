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





















































