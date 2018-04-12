### 枚举

* 语法规范： 枚举类型第一个字母小写

```
enum Direction {
    case  east
    case  west
    case  south
    case  north
}

Direction.west

// 或者
enum Fangxiang {
    case  east, west, south, north
}
```

* 给枚举赋值

> Swift中，枚举默认情况下，不表示任何类型就是一个标识
>
> 枚举类型可以是字符串/字符/整形/浮点型

```
enum Direction: Int { //赋值类型，必须给枚举写类型
    case  east = 2
    case  west = 3
    case  south = 4
    case  north = 5
}

// rawValue 代表的是一个枚举对应的原始值
// 枚举值 -> 原始值
var rv = Direction.east.rawValue
//原始值 -> 枚举值
let z  = Direction(rawValue: 4)

//枚举值绑定字符串
enum Path: String {

    case cache = "cache"
    case docment = "documnet"
}

print(Path.cache.rawValue)
```

### 结构体

##### 1、结构体概念

* 结构体是由一系列有相同类型或者不同类型的数据构成的数据集合
* 结构体指是一种数据结构
* 结构体是值类型，在方法中传递时是指传递

##### 2、结构体格式

* struct 结构体名称{属性和方法}
* 无论是枚举还是结构体都可以写方法
* 类型方法：static func  实例方法：func  \(区别：参考OC中的类方法和实例方法，类调用和类对象调用\)
* 类型属性  实例属性  \(区别：参考OC中的类属性和实例属性，类调用和类对象调用

```
struct Point {
    //实例属性
    var x: Double
    var y: Double

    //实例方法
    func distance() {
        print("距离",x,y)
    }

    // 类型属性
    static var z: Double = 0

    static func dis () {
        print("xxx",z)
    }
}
// 类属性，类方法的调用
Point.z = 1
Point.dis()

// 实例属性，实例方法的调用
var p  = Point(x: 1,y: 2)
p.distance()
p.x = 3
p.distance()
```

##### 3、结构体的扩充函数

* 默认情况下，结构体会自动创建一个“琢一构造器”---&gt;目的就是让所有"非可选成员"都能有值
* 扩充构造函数

```
struct Point {

    //非可选，永远不会为nil
    var x: Double
    var y: Double
    var z: Double?

    //自定义函数 != 普通函数
    //必须使用init 作为名称
    // 在构造函数内部，必须保证，所有的非可选熟悉感，必须有值
    // 如果我们现在自定定义的构造函数，那么系统自动生成的琢一构造器就没有了

    init(x: Double, y: Double) {
        self.x = x
        self.y = y
    }

    init(x: Double, y: Double, z: Double) {
        self.x = x
        self.y = y
        self.z = z
    }
}
//我们直接使用时系统提供的"构造函数"=="构造实例函数"
//let p = Point(x:1,y:2)
//p.z = nil

let p = Point(x: 1, y: 2)
let p1 = Point(x: 1, y: 2, z: 3)

// 系统默认生成的构造函数→"逐一构造器"
// 逐个给里面所有的非可选属性赋值,目的,就是为了保证当一个实例创建好之后,里面所有的非可选属性,都有值
```



