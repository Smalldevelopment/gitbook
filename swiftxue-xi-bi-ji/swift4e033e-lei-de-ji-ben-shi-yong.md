#### 类的介绍

> 注意：当一个实例对象被创建好之后,必须保证里面所有的非可选属性都有值

```
// swift 中,类是可以不继承父类的,那它本身就是rootClass
class Person{

 //方案一:
var name : String
 init(){
 self.name = ""
 }
 init(name:String){
 self.name = name
 }
 //方案二:
var name : String?
//方案三:
var name : String = ""
 //可以写属性和方法
 //属性:实例属性,类型属性
 //方法:实例方法,类型方法
}
//类,默认情况下,不会生成逐一构造函数(目的:保证所有非可选属性有值)
//默认情况下,不能保证,所有的非可选属性有值
//当一个实例对象被创建好之后,必须保证里面所有的非可选属性都有值
//方案一:在构造函数中下手
//方案二:把非可选改成可选
//方案三:把非可选赋默认值
let p = Person()

class Person1:NSObject{
    var name: String 
    //若继承NSObject则必须重写构造方法
    override init(){ 
         self.name = ""
     }
}
```

```
class Stu {
    //属性 实例属性 类型属性
    //类型属性
    static var personCount :Int = 0
    //实例属性(存储属性,计算属性)
    //存储属性:可以直接用来存储数值的属性
    var name :String = ""
    var age :Int = 0
    var scoral :Double = 0
    var scoral2 :Double = 0
    //计算属性:并不是直接用来存储数值的,他是通过某写计算得来的数值
    var avgScoral :Double = {
        get{
            return (scoral+scoral2)/2
        }
    } 
    init(){
        Stu.personCount+=1
    }
    deinit {
        Stu.personCount-=1
    }
    //实例方法
    func text() {
     print("当前有\(Stu.personCount)人")
    
    }
    //类方法
    //static修饰不能被重写
    static func PrintCount{
        
        print("当前有\(Stu.personCount)人")

    }
}
var p :Stu? = Stu()
var p1 :Stu? = Stu()
var p2 :Stu? = Stu()
Stu.personCount
p = nil
p1 = nil
p2 = nil
Stu.personCount
```

###### 析构函数

```
class Stu: NSObject{

    var name:String = ""
    var age:Int = 0
    // 析构函数 == OC 中的dealloc
    deinit {
        print("对像被释放了")
    }
}

var s:Stu? = Stu()
s = nil
```



