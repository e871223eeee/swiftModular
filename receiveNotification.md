**Notification簡單範例**  


`新增MyReceiver`
```swift
import Foundation

class MyReceiver: NSObject {
    @objc func receiveNotification(_ notification: Notification){
        if let n = notification.object as? Int{
            //針對不同的key做不同的事
            if notification.name.rawValue == "MYKEY"{
                print(n)
            }
        }
    }
}
```

`要觀察的ViewController`
```swift
import UIKit

class ViewController: UIViewController {
    
    @objc let receiver = MyReceiver()
    var n = 0
    
    override func viewDidLoad() {
        super.viewDidLoad()
        //註冊被觀察者
        NotificationCenter.default.addObserver(receiver, selector: #selector(MyReceiver.receiveNotification(_:)), name: NSNotification.Name("MYKEY"), object: nil)
    }
    
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        //對特定事件觸發
        NotificationCenter.default.post(name: NSNotification.Name("MYKEY"), object: Int(n))
        n = n + 1
    }
}
```
