####  1. guard 语句

1. guard 是swift2.0新增语法
2. guard语句必须带有else语句，语法如下

> 当条件表达式为true时，跳过else语句中的内容，执行语句组内容
>
> 当条件表达式为false时，执行else语句内容，跳转语句一般是reture,break,continue,throw

```
//如果一个成年人带了省份证才能上网
 func cherk(age:Int hasCard:Bool){
        if age >= 18{
           if hasCard{
           print("老板,开个机")
           }else{
           print("回家拿身份证")
           }
       else{
           print("未成年不能上网")
     }
 }

 func cherkGuard(age:Int hasCard:Bool){
        guard age >= 18 else {
        print("老板,开个机")
        return
        }
        guard hasCard else {
        print("回家拿省份证")
        return
        }
        print("未成年不能上网")
}
```

#### 2、switch的基本使用

 **OC中**

> switch后面必须加（）
>
> case后面只能加一个条件
>
> case会有穿透效果
>
> 可以不写default
>
> default位置可以随便放
>
> case中定义变量需要加大括号，否则作用域会混乱
>
> 不能判断对象或者浮点型类型，只能判断整数

 **Swift**

> switch后面可以不加（）
>
> case后面能跟多个条件用逗号隔开
>
> case不会有穿透效果，要穿透需要在后面加fallthrough
>
> 不可以不写default,位置也必须放在最后
>
> case中定义变量不需要加大括号
>
> 能判断对象或浮点型，整数

 元祖匹配

```
let point = (10,15)

switch point {
case (0,0):
    print("在坐标原点")
case (1...10,1...10):
    print("在1...10之间")
case (10,15):
    print("在X轴上")
default:
    print("其他")
}
```

 值绑定

```
switch point {
case (var x,0):
    print("x=\(x)")
case (10,var y):
    print("y=\(y)")
case var(x,y):
    print("x=\(x) y=\(y)")
default:
    print("其他")
}
```

 根据条件绑定

```
let po = (100,10)

switch po {
case var(x,y) where x > y:
    print("x=\(x) y=\(y)")
default:
    print("其他")
}
```

#### 3、区间匹配

* **区间匹配  概念：通常描述的是数字区间分为闭区间和半开半闭区间**

```
0...10//闭区间代表区域在0~10
0..<10//半开半闭区间代表区域在0~、
```

 **区间操作**

> 交集：clamped
>
> 是否重叠：overipas
>
> 判断包含：contains
>
> 是否为空: isEmpty

#### 4、for循环

```
for var x in 0..<10 {
    print(x)
}
```

#### 5、while循环和do while循环

```
while i > 0 {
    i -= 1
    print(i)
}
//do while 循环  swift中不用do ,do 在swift转给你有特殊含义，用于捕捉异常
repeat{
    i += 1
    print(i)
}while i < 10
```



