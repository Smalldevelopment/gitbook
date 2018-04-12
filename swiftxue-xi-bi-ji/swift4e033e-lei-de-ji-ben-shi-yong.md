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



