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







































