强制解包的四种方法：

一、直接判断+解包

二、可选绑定

三、guard

四、空合运算符



#### KVO 的使用

```
class Stuu: NSObject {
    var name: String?
    var age: Int?
    
    init(dic:[String: Any]) {
//        let name = dic["name"] as? String ?? ""
//        let age = dic["age"] as? Int ?? 0
//        self.name = name
//        self.age = age
        
        //kvc实现  再次之前必须调用父类的Init的进行初始化
        super.init()
        setValuesForKeys(dic)
        
    }
}

let dic1: [String: Any] = ["name":"zs", "age": 18]

let stu = Stuu(dic: dic1)
```

#### 类的析构函数

```
class Stu: NSObject {
    var name: String?
    var age: Int?
    var score: Int?
    
    //析构函数 == OC 里面的dealloc
    deinit {
        print("对象释放了")
    }
    //OC 里面ARC 管理是什么对象？ OC对象
    
}

var s: Stu? = Stu()
```



