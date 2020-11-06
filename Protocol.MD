**Protocol簡單範例**  


`委任的目標`
```swift
import Foundation
import UIKit

class tset: UIView {
    //代理
    var delegate:tsetListener?
    
    
    
    func a(){
          delegate?.a?(int: 10)
    }
    
    func b(){
          delegate?.b?(bool: true)
    }
      
    func c(){
          delegate?.c?(string: "test")
    }
    
    
    
//MARK: - TextField事件的代理
@objc protocol tsetListener:NSObjectProtocol {
    
    //目前字數
    @objc optional func a(int:Int)
    
    //是否有內容
    @objc optional func b(bool:Bool)
    
    //目前內容
    @objc optional func c(string:String)
}
}
```


`委任的畫面`
```swift
import UIKit

```swift
class testVC: UIViewController {
     override func viewDidLoad() {
        super.viewDidLoad()
        //委任
        tset.delegate = self
     }
}

extension testVC:tsetListener{
    func a(int:Int) {
        print(int)
    }
    
    func b(bool:Bool) {
        print(bool)
    }
    
    func c(string:String){
        print(string)
    }
}

```
