焦点：能否与用户进行交互

##### 5程序启动原理

* 执行main函数
* 执行UIApplicationMain，创建UIApplication的对象，并设置UIApplication的代理

```
int main(int argc, char * argv[]) {
    @autoreleasepool {
        //第三个参数 :设置应用程序对象的名称UIApplication或者它的子类，如果是nil默认是UIApplication
        //第四个参数：设置代理
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
```

* 开启一个事件循环（主运行循环，死循环，保证程序不会退出）
* 去加载`info.plist`（判断`info.plist`当中有没有`main`参数,如果有加载`main.storyBoard`）
* 应用程序启动完毕（通知代理应用程序启动完毕）

#### Window

1. 从iOS9 之后，如果添加多个窗口，控制器他会自动把状态栏给隐藏掉  解决办法 把状态栏给应用程序来管理（在`info,plist`中添加`View controller-based status bar appearance` 为`NO`）
2. 窗口的层级   `UIWindowLevelNormal  <   UIWindowLevelStatusBar  < UIWindowLevelAlert`
3. initWithName:如果制定了特定的名称xib会去加载指定的xib

> 如果指定的是nil
>
> 1、判断有没有当前控制器相同名称的xib，如果有，自动加载跟他仙童名称的xib
>
> 2、如果没有跟他仙童名称的xib,自动加载跟它相同名称并且去掉`controller`的xib文件
>
> 3、`init`底层自动调用了`initWithName`

#### LoadView底层实现（- \(void\)loadView）

1. 先判断当前控制器是不是从storyboard当中加载，如果是从storyBoard当中加载的控制器，那么它就会从storyBoard当中加载的控制器的View，设置当前控制器的View
2. 单钱控制器是不是从xib中加载的，如果是从xib中加载的，把xib当中指定的view设置为当前控制器的View
3. 如果也不是从xib中加载的，它会创建一个空白的View\(空白的View是背景颜色是`cleanColor` 而不是其`alpha`为0,如果一个空间是透明的，那么它不能够接受事件\)

> 骚操作：当界面是一张图片或者网页的时候，可以在loadView中直接用imageView或者webView 当成控制器的View，

#### 控制器的懒加载

```
- (UIView *)view
{
    if (!_view) {
        [self loadView];
        [self viewDidLoad];
    }
    return _view;
}
```

#### KVC

```
 [model setValuesForKeysWithDictionary:dic];
 [model enumerateKeysAndObjectsUsingBlock:^(id_Nonnull key, id_Nonnull obj, BOOL * _Nonnull stop) {
 [model setValue:obj forKey:key];
 }];
```

setValue:forkey实现原理

1、先查看有没有对应key值得set方法,如果有set方法，就给对应的属性赋值

2、如果没有set方法，就去查看有没有跟key值相同并且带有下划线的成员属性，如果有的话，就给带有下划线的成员属性赋值

3、如果咩有跟key值相同并且带有下划线的成员属性，还会去找有没有跟key值相同的名称的成员属性，如果有，就给他赋值

4、如果没有直接报错

#### 字符串copy和strong

字符串用copy原因外界修改不会改变字符串的值，在strong中的set方法是 \_name = name;而在copy方法中是\_name = \[name copy\];如果name是可变字符串就执行copy，如果不是就直接赋值

#### 导航控制器

```
 UINavigationController *nav= [[UINavigationController alloc] initWithRootViewController:nil];
    [nav pushViewController:nil animated:YES];
```

其中`initWithRootViewController`就是调用`[nav pushViewController:nil animated:YES]`方法来进行操作的

#### View的声明周期

1. ViewDidLoad\(懒加载,如果没释放只会调用一次\)
2. ViewWillAPPear
3. VIewWillLayoutSubviews
4. VIewDidLayoutSubviews
5. VIewWillLayoutSubviews
6. VIewDidLayoutSubviews
7. ViewDidAppear

#### storyboard跳转

1. 连线跳转 `[self performSegueWithIdentifier:@"" sender:nil];`

**performSegueWithIdentifier底层实现**

1、到storyboard当中去查找有没有给定标识的segue

2、根据指定的标识，创建一个UIStoryboardSegue对象，把当前的控制器设置为UIStoryboardSegue的源控制器属性赋值（segue.sourceViewController），

3、UIStoryboardSegue对象再去创建其目标控制器，给UIStoryboardSegue对象的目标控制器赋值（segue.destinationViewController）

4、调用当前控制器的prepareForSegue:方法，会告诉用户，当前的线已经准备好了

5、 执行\[segue perform\];方法，其实也就是执行 \[segue.sourceViewController.navigationController pushViewController:segue.destinationViewController animated:YES\];



#### block

`inline` 是快速敲出一个block的快捷

block类型       
//nameBlock 类型名

`typedefvoid(^nameBlock) (NSString *str);`

block作用：跟函数和方法很想，其实就是用来保存一段代码，等到恰当的时候再去使用

#### 应用沙盒

* Documents,保存应用运行时生成的需要持久化的数据，itunes同步设备时会备份该目录
* tem保存应用运行时所需要的临时数据，使用完毕后将相应的文件从该目录删除，应用没有运行时，系统也可能会清楚该目录下的文件，itunes同步设备时不会备份该目录
* library/caches保存应用运行时声称的需要持久化的数据，itunes同步设备时不会备份
* library/preference 保存应用所在的所有偏好设置，itunes同步设备会备份该目录

 ` NSHomeDirectory()` 获取应用沙盒地址

* 获取cache文件路径
* NSSearchPathForDirectory 搜索的目录

* NSSearchPathDomainMask：搜索范围 NSUserDomainMask表示在用户手机上查找

* BOOL expandTilde 是否展开路径 如果没有展开，应用沙盒路径就是~

 `NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask,YES);`



#### 存储方式

1. plist存储
2. 偏好设置存储

`NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults]`

`[userDefaults setObject:@"zs"forKey:@"name"];`

`[userDefaults synchronize]；`

 3.自定义归档

```
   调用方法
     person *p =[[person alloc] init];
    NSString *cache = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];
    NSString *filepath =[cache stringByAppendingPathComponent:@"person.data"];
    
    [NSKeyedArchiver archiveRootObject:p toFile:filepath];
    
    person *pr=[NSKeyedUnarchiver unarchiveObjectWithFile:filepath];
    
    实现协议
    //归档的时候调用
- (void)encodeWithCoder:(NSCoder *)aCoder
{
    [aCoder encodeObject:_name forKey:@"name"];
    [aCoder encodeInteger:_age forKey:@"age"];
}
//解档的时候调用
//只要父类准售了NSCoding,就调用initWithCoder否则调用[super init]
//解析文件的时候回调用该方法
- (instancetype)initWithCoder:(NSCoder *)aDecoder
{
    if (self = [super init]) {
        
        self.name =[aDecoder decodeObjectForKey:@"name"];
        self.age = [aDecoder decodeIntegerForKey:@"age"];
        
    }
    return self;
}
```

TableView的storyBoard中静态cell行数已经固定了，不能再代码中再做更改



#### 模态弹出modal

```
    TwoViewController *twoVC =[[TwoViewController alloc] init];
    
    //模态变换
    [self presentViewController:twoVC animated:YES completion:nil];
        
    //原理：就是把modal出来的控制器的View加到窗口最上面，期间这个控制器需要被强引用，
    因为到了下一个控制器需要执行控制器上面类方法，所以不能被销毁，系统用presentViewController这个属性持有的
    //实现步骤
    //获取窗口
    UIWindow *window = [UIApplication sharedApplication].keyWindow;
    //添加控制器的View
    [window addSubview:twoVC.view];
    //往上平移一个控制器的高度
    twoVC.view.transform =CGAffineTransformMakeScale(0, self.view.frame.size.height);
    
    [UIView animateWithDuration:0.5 animations:^{
        //清除型变
        twoVC.view.transform  = CGAffineTransformIdentity;
    }];
    
    
    
    [self dismissViewControllerAnimated:<#(BOOL)#> completion:<#^(void)completion#>];
```

























