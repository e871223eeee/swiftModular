**Notification簡單範例**  


`新增MyReceiver`
```swift
import Foundation

class MyReceiver: NSObject {
    //監聽
    @objc func receiveNotification(_ notification: Notification){
        //判斷目標對象對不對
        if notification.name == .TEST{
            guard let userInfo = notification.userInfo,
                let mode = userInfo["mode"] as? testEnum else {
                    print("No userInfo found in notification")
                    return
            }
            
            //針對不同的mode做不同的事
            switch mode {
            case .a:
                print("a")
            case .b:
                print("b")
            case .c:
                print("c")
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
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        //註冊被觀察者
        NotificationCenter.default.addObserver(receiver, selector: #selector(MyReceiver.receiveNotification(_:)), name: .TEST, object: nil)
    }
    
    @IBAction func a(_ sender: Any) {
        NotificationCenter.default.post(name: .TEST, object: nil, userInfo: ["mode":testEnum.a])
    }
    @IBAction func b(_ sender: Any) {
        NotificationCenter.default.post(name: .TEST, object: nil, userInfo: ["mode":testEnum.b])
    }
    @IBAction func c(_ sender: Any) {
        NotificationCenter.default.post(name: .TEST, object: nil, userInfo: ["mode":testEnum.c])
    }
    
}

```


`SetNSNotification`
```swift
import Foundation

extension NSNotification.Name {
    static let TEST = NSNotification.Name("TEST")
}
```


`NotificationUserInfoEnum`
```swift
import Foundation

//MARK: - 列舉
enum testEnum{
    case a
    case b
    case c
}
```
