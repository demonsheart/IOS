## IOS Note

### 关于导航栏底部问题

```swift
UINavigationBar.appearance().isTranslucent = false // 导航条背景是否透明

当上面的值为false时，加入的子视图会自动从导航栏下方开始，而不用自己计算
```

### 关于Height

```swift
let statusHeight: CGFloat = view.window?.windowScene?.statusBarManager?.statusBarFrame.height ?? 0
let navigationHeight: CGFloat = self.navigationController?.navigationBar.frame.height ?? 0
let tabbarHeight: CGFloat = self.tabBarController?.tabBar.frame.size.height ?? 0

var safeInsets: UIEdgeInsets {
    if #available(iOS 11.0, *) {
        return UIApplication.shared.mainWindow?.safeAreaInsets ?? UIEdgeInsets.zero
    } else {
        return UIEdgeInsets.zero
    }
}
```



### 隐藏tabbars

```swift
 let childVC = ChildViewController()
 childVC.hidesBottomBarWhenPushed = true // 一般而言，子视图隐藏Bar即可
 self.navigationController?.pushViewController(childVC, animated: true)
```



### 回调

* ParentVC

```swift
childVC.callBack = { [weak self] (str) in
  // do something
}
```

* ChildVC

```swift
var callBack: ((String) -> Void)?
```

在这里 ParentVC强引用了ChildVC，因此在定义ChildVC的callBack捕获ParentVC的self时，要用weak关键字表示弱引用，否则就会产生循环强引用。

### layer mask & gradient 渐变填充

* 图层结构

  > **self.layer -> gradient ---mask---> shapeLayer**
  >
  > **特别注意shapeLayer的坐标原点是以gradient为基准的**
  >
  > ```swift
  > self.layer.addSublayer(gradient)
  > gradient.mask = shapeLayer
  > ```

* 关于mask

  The area:  shape's alpha == 0:  cannot see gradient

  The area:  shape's alpha == 1:  can see gradient

  总而言之，shape层作为mask层，有颜色的地方会被gradient填充，没颜色的地方照旧。



### Alamofire例子

```swift
Alamofire.request("http://data.taipei/opendata/datalist/apiAccess?scope=resourceAquire&rid=e6831708-02b4-4ef8-98fa-4b4ce53459d9").responseJSON(completionHandler: { response in
     if response.result.isSuccess {
         // convert data to dictionary array
         if let result = response.value as? [String: AnyObject] {
             let dataList: [[String : Any]]? = result["result"]?["results"] as? [[String : Any]]
             for data in dataList! {
                 print("locationName: \(data["locationName"]!)") // 所在縣市
                 print("parameterName1: \(data["parameterName1"]!)") // 天氣
                 print("startTime: \(data["startTime"]!)") // 起始時間
                 print("endTime: \(data["endTime"]!)") // 結束時間
                 print()
             }
         }
     } else {
         print("error: \(response.error)")
     }
 })
```

