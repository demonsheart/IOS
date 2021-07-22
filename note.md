## IOS Note

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
  > ```swift
  > self.layer.addSublayer(gradient)
  > gradient.mask = shapeLayer
  > ```

* 关于mask

  The area:  shape's alpha == 0:  cannot see gradient

  The area:  shape's alpha == 1:  can see gradient

  总而言之，shape层作为mask层，有颜色的地方会被gradient填充，没颜色的地方照旧。