**XIB中利用navigation跳頁與傳值**



`新開一個swift文檔`
```swift
import Foundation
import  UIKit

class BaseViewController: UIViewController {
    
    //push
    func push(_ nextVC: UIViewController){
        navigationController?.pushViewController(nextVC, animated: false)
    }
    
    //push 跳頁時 刪掉舊的畫面
    func push_deleteCurrentVC(_ CurrentVC:UIViewController, _ nextVC: UIViewController){
        
        navigationController?.pushViewController(nextVC, animated: false)
        

        //刪除上一個畫面
        let arrayVC = NSMutableArray.init(array: (self.navigationController?.viewControllers)!)
        for vc in arrayVC {
            if (vc as! UIViewController) == CurrentVC  {
                arrayVC.remove(vc)
                break;
            }
        }
        
        self.navigationController?.viewControllers = arrayVC as! [UIViewController]
    }
    
    //舊的畫面被叫出時無法傳值 待解決
    //pop
    func pop(_ nextVC: UIViewController){
        //目的ViewController是否已經生成過了
        var isRegisterVC = false
        let arrayVC = NSMutableArray.init(array: (self.navigationController?.viewControllers)!)

        //如果生成過了就叫出來
        for vc in arrayVC{
           if (vc as! UIViewController) == nextVC  {
            self.navigationController?.popToViewController(vc as! UIViewController, animated: false)
                isRegisterVC = true
                break;
           }
        }

        //如果沒有就直接push
        if !isRegisterVC{
            navigationController?.pushViewController(nextVC, animated: false)
        }
    }
    
    //舊的畫面被叫出時無法傳值 待解決
    
    //pop 跳頁時 刪掉舊的畫面
    func pop_deleteCurrentVC(_ CurrentVC:UIViewController, _ nextVC: UIViewController){
        //目的ViewController是否已經生成過了
        var isRegisterVC = false
        var arrayVC = NSMutableArray.init(array: (self.navigationController?.viewControllers)!)

        //如果生成過了就叫出來
        for vc in arrayVC{
           if (vc as! UIViewController) == nextVC  {
            self.navigationController?.popToViewController(vc as! UIViewController, animated: false)
                isRegisterVC = true
                break;
           }
        }

        //如果沒有就直接push
        if !isRegisterVC{
            navigationController?.pushViewController(nextVC, animated: false)
        }
        
        
        //刪除上一個畫面
        arrayVC = NSMutableArray.init(array: (self.navigationController?.viewControllers)!)
        for vc in arrayVC {
            if (vc as! UIViewController) == CurrentVC  {
                arrayVC.remove(vc)
                break;
            }
        }
        
        self.navigationController?.viewControllers = arrayVC as! [UIViewController]
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
