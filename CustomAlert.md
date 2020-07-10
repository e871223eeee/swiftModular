**自定義AlertView**

`開新檔案CustomAlert`
```swift
import Foundation
import UIKit

class CustomAlert {
  //警告提示框
  class func okAlert(_ title: String?,
                     _ message: String?,
                     _ view : UIViewController,
                     _ actionFunc :  (() -> Void)?) {
    DispatchQueue.main.async {
      // 建立一個提示框
      let alertController = UIAlertController(title: title,
                                                message: message,
                                                preferredStyle: .alert)
      
      // 建立[確認]按鈕
      let Action = UIAlertAction(title: "確認",
                                  style: .default,
                                  handler: {(action: UIAlertAction) -> Void in actionFunc?()})
      
      //加入按鈕
      alertController.addAction(Action)
      
      // 顯示提示框
      view.present(alertController,
                     animated: true,
                     completion: nil)
      }
  }
  
  //確認提示框
  class func yesNoAlert(_ title: String?,
                        _ message: String?,
                        _ view : UIViewController,
                        _ actionFunc :  ((Bool) -> Void)?) {
    DispatchQueue.main.async {
      // 建立一個提示框
      let alertController = UIAlertController(title: title,
                                                message: message,
                                                preferredStyle: .alert)
      
      // 建立[確認]按鈕
      let yesAction = UIAlertAction(title: "確認",
                                      style: .default,
                                      handler: {(action: UIAlertAction) -> Void in actionFunc?(true)})
                                      
      // 建立[取消]按鈕
      let noAction = UIAlertAction(title: "取消",
                                    style: .default,
                                    handler: {(action: UIAlertAction) -> Void in actionFunc?(false)})
                                    
      //加入按鈕
      alertController.addAction(yesAction)
      alertController.addAction(noAction)
      
      // 顯示提示框
      view.present(alertController,
                     animated: true,
                     completion: nil)
    }
  }
    
  //輸入提示框
  class func TFAlert(_ title: String?,
                     _ message: String?,
                     _ TFtext:String?,
                     _ view : UIViewController,
                     _ actionFunc :  ((Bool, String) -> Void)?) {
    DispatchQueue.main.async {
      // 建立一個提示框
      let alertController = UIAlertController(title: title,
                                                message: message,
                                                preferredStyle: .alert)
            
      alertController.addTextField { (textField) in
                textField.placeholder = "\(TFtext!)"
                textField.isSecureTextEntry = false}
            
      // 建立[確認]按鈕
      let yesAction = UIAlertAction(title: "確認",
                                      style: .default,
                                      handler: {(action: UIAlertAction) -> Void in actionFunc?(true, alertController.textFields![0].text!)})
      
      // 建立[取消]按鈕
      let noAction = UIAlertAction(title: "取消",
                                    style: .default,
                                    handler: {(action: UIAlertAction) -> Void in actionFunc?(false, "")})
            
      //加入按鈕
      alertController.addAction(yesAction)
      alertController.addAction(noAction)
      
      // 顯示提示
      view.present(alertController,
                    animated: true,
                    completion: nil)
    }
  }
}
```

`警告提示框使用`
```swift
//確認後沒有要做額外事情
CustomAlert.okAlert("Your Title", "Your message", self, nil)
                
//確認後要做額外事情
CustomAlert.okAlert("Your Title", "Your message", self) {
  //要做的事情          
}
```

`確認提示框使用`
```swift
//沒有要做事
CustomAlert.yesNoAlert("Your Title", "Your message", self, nil)
                
//根據不同的按鍵做不同的事
CustomAlert.yesNoAlert("Your Title", "Your message", self) { (value) in
  if value{
    //確認要做的事情
  }else{
    //取消要做的事情
  }
}
```

`輸入提示框使用`
```swift
//沒有要做事
CustomAlert.TFAlert("Your Title", "Your message", "Your TextField Text", self, nil)
                
//根據不同的按鍵做不同的事,同時能抓取AlertView中TextField輸入的內容
CustomAlert.TFAlert("Your Title", "Your message", "Your TextField Text", self) { (BoolValue, YourInputText) in
  if BoolValue{
    //確認要做的事情
  }else{
    //取消要做的事情
  }
                    
   //印出你輸入的內容
   print(YourInputText)
}
```
