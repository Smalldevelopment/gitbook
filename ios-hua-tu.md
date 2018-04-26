## 前言

应该是没有前言的，但是，尼玛，写了一上午的画图。第二天起来一看全没了，不得不吐槽下Gitbook，又得重复劳动…………

---

> ##### 以下操作在均在自定义View的drawRect方法中

#### 画线条

* 获取上下文（在drawRect方法中系统已经帮我们创建好一个跟View相关联的上下文了，我们直接获取就行，其他的我们还要自己开启和关闭）

```
CGContextRef  ctx = UIGraphicsGetCurrentContext();
```

* 绘制路径

```
UIBezierPath *path = [UIBezierPath bezierPath];
//设置起点
[path moveToPoint:CGPointMake(50, 200)];
//添加一条线的终点
[path addLineToPoint:CGPointMake(280, 50)];

//画第二条线
[path moveToPoint:CGPointMake(100, 200)];
[path addLineToPoint:CGPointMake(250, 100)];

//把上一条线的终点当做下一天线的起点
[path addLineToPoint:CGPointMake(200, 200)];
```

> 如果我们想对线条做一些操作，可以用以下这几种方式
>
> ```
> //上下文的状态(线宽颜色)
> CGContextSetLineWidth(ctx, 10);
> //线的交点样式
> CGContextSetLineJoin(ctx, kCGLineJoinRound);
> //线的两头样式
> CGContextSetLineCap(ctx, kCGLineCapRound);
> //设置颜色(要和下文渲染和填充用一样的)
> //[[UIColor redColor] setStroke];
> //一般用这个设置颜色
> [[UIColor redColor] set];
> ```

* 把绘制内容添加到上下文中

```
// CGPathRef:CoreGraphics框架下的
// UIBezierPath:UIKit框架下的
CGContextAddPath(ctx, path.CGPath);
```

* 把上下文的内容显示到View上（渲染到View的Layer上，有两种渲染方式stroke描边和 fill填充）

```
CGContextStrokePath(ctx);
```

![](/assets/Snip20180425_1.png)

> ##### 如果我们想要在一个View上画出不同的线条该怎么操作呢,其实有个上下文状态站可以为我们解决（可以保存多个和取出多个）
>
> ```
>  CGContextSaveGState(ctx);//保存
>  CGContextRestoreGState(ctx);//取出
> ```

```
    //1.获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    //2.描述路径
    UIBezierPath *path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(20, 150)];
    [path addLineToPoint:CGPointMake(280, 150)];

    //3.把路径添加到上下文当中.
    CGContextAddPath(ctx, path.CGPath);

    //保存当前上下文的状态(第一次保存)
    CGContextSaveGState(ctx);

    //设置上下文的状态
    CGContextSetLineWidth(ctx, 10);
    [[UIColor redColor] set];

    //保存当前上下文的状态(第二次保存)
    CGContextSaveGState(ctx);

    //4.把上下文当中的内容渲染View
    CGContextStrokePath(ctx);

    UIBezierPath *path2 = [UIBezierPath bezierPath];
    [path2 moveToPoint:CGPointMake(150, 20)];
    [path2 addLineToPoint:CGPointMake(150, 280)];
    //把路径添加到上下文当中.
    CGContextAddPath(ctx, path2.CGPath);

    //从上下文状态栈当中恢复上下文的状态（取出保存两次的）
     CGContextRestoreGState(ctx);
     CGContextRestoreGState(ctx);

    //把上下文当中的内容渲染View
    CGContextStrokePath(ctx);
```

![](/assets/Snip20180426_12.png)

#### 画曲线

```
 CGContextRef ctx = UIGraphicsGetCurrentContext();
 UIBezierPath *path = [UIBezierPath bezierPath];
 [path moveToPoint:CGPointMake(50, 280)];
 [path addQuadCurveToPoint:CGPointMake(250, 280) controlPoint:CGPointMake(50, 50)];
    
 CGContextAddPath(ctx, path.CGPath);
  CGContextStrokePath(ctx);
```

#### ![](/assets/Snip20180425_4.png)

#### 画矩形

```
CGContextRef ctx = UIGraphicsGetCurrentContext();
[[UIColor redColor] set];
//矩形
//  UIBezierPath *path = [UIBezierPath bezierPathWithRect:CGRectMake(50, 50, 100, 180)];
//圆角矩形
UIBezierPath *path = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(50, 50, 180, 100) cornerRadius:50];

CGContextAddPath(ctx, path.CGPath);

// CGContextStrokePath(ctx);
CGContextFillPath(ctx);
```

![](/assets/Snip20180425_3.png)

#### 画椭圆  （快速画图\)

```
//椭圆
UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(50, 50, 100, 50)];
//使用UIBezier提佛那个的绘图方法
// [path fill];
//只有在drawRect方法中才可以，因为默认调用了下面的获取上下门的方法
[path stroke];
```

![](/assets/Snip20180425_5.png)

#### 画弧

```
//画弧
//center 弧所在的圆心
//园的半径
//开始角度 最右侧开始 可以为-数
//截止角度
//YES 顺时针 NO逆时针
CGPoint center = CGPointMake(rect.size.width * 0.5, rect.size.height * 0.5);

UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:50 startAngle:0 endAngle:-M_PI_2 clockwise:NO];
//添加一条线到圆心
[path addLineToPoint:center];
//关闭路径closePath: 从路径重点连接一根线到路径的起点
[path closePath];

[path stroke];
```

![](/assets/Snip20180425_9.png)

> 小记：如果想主动调用`drawRect：`方法可以调用View的`setNeedsDisplay`方法

画大饼

```
//画第一个扇形
CGPoint center = CGPointMake(rect.size.width * 0.5, rect.size.height * 0.5);
CGFloat radius = rect.size.width * 0.5 - 40 ;
CGFloat endA = 0.25 * M_PI * 2;
UIBezierPath * path = [UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle: 0 endAngle:endA clockwise:YES];

[[UIColor redColor] set];
[path addLineToPoint:center];
[path fill];


CGFloat endA2 = endA + (0.25 * M_PI * 2) ;
UIBezierPath * path2 =[UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle:endA endAngle:endA2 clockwise:YES];

[[UIColor greenColor] set];
[path2 addLineToPoint:center];
[path2 fill];

CGFloat endA3 = endA2 + (0.5 * M_PI * 2) ;
UIBezierPath * path3 =[UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle:endA2 endAngle:endA3 clockwise:YES];

[[UIColor yellowColor] set];
[path3 addLineToPoint:center];
[path3 fill];
```

##### ![](/assets/Snip20180426_10.png)



