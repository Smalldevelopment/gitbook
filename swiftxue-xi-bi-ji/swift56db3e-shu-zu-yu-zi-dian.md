#### 数组

* 可变数组

```
let arr: [Any] = [1,2,3]
```

* 不可变数组

```
let arr: [Any] = [1,2,3]

var array: [Any] = [1,2,3]

array.append(4)
array.insert("你好", at: 2)
array + [7,8]
array.remove(at: 1)
array.removeFirst(2)//删除前面两个
array.removeLast(2)//删除后面两个
array += [1,2,3]
array.removeSubrange(1...2)//删除某个区间
array[0] = 2 //改
array.removeAll()

array += [1,2,3]
array.count //个数
array.capacity //容量
```

* 数组遍历

```
for j in 0..<array.count {
    print(array[j])
}
for value in array {
    print(value)
}
//遍历某个区间
for value in array[0...3]{
     value
}
//遍历同时取下标
for (index,value) in array.enumerated() {
    print(index,value)
}
```

#### 字典

```
 let dic : [String : Any] = ["a":1, "b":"2"] //不可变
 var dic : [String : Any] = ["a":1, "b":"2"] //可变 
```

* 增

```
dic["c"] = 2
//或则
dic.updateValue(2,forKey:"c")
//以上两种如原字典中已存此key,则修改value ,否则新增该键值对
```

* 删

```
dic.removeValue(forKey:"a")
//通过索引删除
let index = dic.index(forKey:"a")
dic.remove(at:index)
```

* 查

```
for (key,value) in dic{
 print (key, ":", value)
}
```



