#### AFN 3.0后模块划分

* **网络通信模块\(AFNURLSessionManager、AFNHTTPSessionManger\)**
* **网络状态监听模块\(Reachability\)**
* **网络通信信息序列化/反序列化模块\(Serialization\)**
* **对于iOS UIKit库的扩展\(UIKit\)**
* **支持文件\(Support Files\)**

       核心模块是AFNURLSessionManager，因为AFN 3.0 是基于NSURLSession 来封装，所以这个类围绕着NSURLSession做了一系列的封装，而其他四个模块，均是为了配合网路通信或者对已有的UIKit的扩展工具包

![](/assets/2702646-10294db19b1aedfd.png.jpeg)



