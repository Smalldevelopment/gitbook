#### 1、字符串的基本使用

* [ ] **OC与Swift字符串的区别**

* OC 中使用字符串类型是NSSting,Swift 中使用的字符串类型是String
* OC中使用的是@“”，Swift用在“”

* [ ] 使用String的原因

* String是一个结构体，性能更高（保存的是直接的值）
* NSString是OC的一个对象，性能略差
* String支持遍历

* [ ] 常见使用

```
var str = "傲慢与偏见"
//获取字符串的长度

str.lengthOfBytes(using:String.Encoding.utf8)//获取字节长度
str.count//获取字符串长度

//字符串遍历
for c in str {
    print(c)
}

//字符串拼接
str + "愚蠢"
```



