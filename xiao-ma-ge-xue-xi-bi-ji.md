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





















































