#### 图片圆角 {#一个被怼的小开发}

##### 具体方法和画图操作基本差不过，主要分为以下几个步骤

1. **开启图片下上文**

```
UIGraphicsBeginImageContextWithOptions(img.size, false, 0.0)
```

1. **描述路径**

```
let path: UIBezierPath = UIBezierPath.init(ovalIn: CGRect(x: 0,y: 0,width: img.size.width,height: img.size.height))
```

1. **开始剪切**

```
path.addClip()
```

1. **开始绘制**

```
img.draw(at: CGPoint(x: 0,y: 0))
```

1. **生成图片**

```
let newImg: UIImage = UIGraphicsGetImageFromCurrentImageContext()!
```

1. **关闭路径**

```
UIGraphicsEndImageContext()
```

> 以下为封装的方法

```
    /// 图片圆角
    ///
    /// - Parameter img: 传入图片
    /// - Returns: 传出新图片
   static func imgeRoundedCorners(img: UIImage) -> UIImage{

        UIGraphicsBeginImageContextWithOptions(img.size, false, 0.0)
        let path: UIBezierPath = UIBezierPath.init(ovalIn: CGRect(x: 0,y: 0,width: img.size.width,height: img.size.height))
        path.addClip()
        img.draw(at: CGPoint(x: 0,y: 0))

        let newImg: UIImage = UIGraphicsGetImageFromCurrentImageContext()!
        UIGraphicsEndImageContext()

        return newImg
    }
```

#### 图片边框圆角

```
    /// 带边框的图片圆角
    ///
    /// - Parameters:
    ///   - img: 图片
    ///   - space: 边框间隔
    ///   - color: 边框颜色
    /// - Returns: 返回生成的新图片
   static func imgBorderRoundedCorners(img: UIImage, space: CGFloat, color: UIColor) -> UIImage{

        let imgSize: CGSize = CGSize(width: img.size.width + 2 * space, height: img.size.height + 2 * space)


        UIGraphicsBeginImageContextWithOptions(imgSize, false, 0.0)
        let path: UIBezierPath = UIBezierPath.init(ovalIn: CGRect(x: 0,y: 0,width: imgSize.width,height: imgSize.height))

        color.set()
        path.fill()

        let path1: UIBezierPath = UIBezierPath.init(ovalIn: CGRect(x: space, y: space, width: img.size.width, height: img.size.height))

        path1.addClip()
        img.draw(at: CGPoint(x: space,y: space))

        let newImg: UIImage = UIGraphicsGetImageFromCurrentImageContext()!
        UIGraphicsEndImageContext()

        return newImg
    }
```

#### 屏幕截屏

```
    /// 截屏
    ///
    /// - Parameters:
    ///   - size: 截屏的大小
    ///   - view: 在那个View上截屏
    /// - Returns: 返回一个图片
    static func imgScreenshots(size: CGSize, in view: UIView) -> UIImage {

        UIGraphicsBeginImageContextWithOptions(size, false, 0.0)
        // 离屏渲染
        view.layer.render(in: UIGraphicsGetCurrentContext()!)

        let imgNew: UIImage = UIGraphicsGetImageFromCurrentImageContext()!

        UIGraphicsEndImageContext()

        return imgNew
    }
```

#### 保存图片的多种方式

##### 1. UIImageWriteToSavedPhotosAlbum函数

```
  //保存到相册的图片对象
  //保存完成功后回调的目标对象
  //保存完成后回调的方法
  //保存完成后，会回调方法的contextinfo中
  UIImageWriteToSavedPhotosAlbum(imgNew, self, #selector(imgSaveDidFinish(img:error:)), nil)

  //保存成功后回调方法
   @objc func imgSaveDidFinish(img: UIImage, error: NSError) {

    }
```

##### 2. 使用Photos框架下的PHPPhotoLibrary类来实现保存

```
  import Photos //导入模块

  PHPhotoLibrary.shared().performChanges({
            //写入相册
            PHAssetChangeRequest.creationRequestForAsset(from: imgNew)
        }) { (success, error) in
            print("成功： \(success)  错误：\(String(describing: error))")
  }
```



