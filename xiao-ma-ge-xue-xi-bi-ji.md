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
* 去加载info.plist（判断info.plist当中有没有main参数,如果有加载main.storyBoard）
* 应用程序启动完毕（通知代理应用程序启动完毕）



