#### DispatchQoS（线程的优先级）  枚举的解释

    public enum QoSClass {

            @available(OSX 10.10, iOS 8.0, *)
            case background  //后台

            @available(OSX 10.10, iOS 8.0, *)
            case utility    // 公共的

            @available(OSX 10.10, iOS 8.0, *)
            case `default`   //默认的

            @available(OSX 10.10, iOS 8.0, *)
            case userInitiated  //用户期望优先级（不要放太耗时的操作）

            @available(OSX 10.10, iOS 8.0, *)
            case userInteractive  //用户交互(跟主线程一样)  

            case unspecified      //不指定 

            @available(OSX 10.10, iOS 8.0, *)
            public init?(rawValue: qos_class_t)

            @available(OSX 10.10, iOS 8.0, *)
            public var rawValue: qos_class_t { get }
        }



