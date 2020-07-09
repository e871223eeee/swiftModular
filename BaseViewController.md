**XIB中利用navigation跳頁與傳值**



`新開一個swift文檔`
```swift
import Foundation
import  UIKit

class BaseViewController: UIViewController {
    func push(_ vc: UIViewController){
        navigationController?.pushViewController(vc, animated: true)
    }
}
```

`在需要跳頁的ViewController使用`
`ViewController_1() 跳頁到 ViewController_2() 並且傳值`

```swift
import UIKit
//需要繼承BaseViewController
class ViewController_1: BaseViewController {
    func goToViewController_2(){
        let vc = ViewController_2()
        vc.value = "your Value"
        self.push(vc)
    }
}
```

```swift
import UIKit

class ViewController_2: UIViewController {
    var value:String = ""
}
```
