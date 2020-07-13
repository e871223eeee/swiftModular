**enum的應用typealias/CaseIterable**
```swift
import UIKit

//自訂型別
typealias mainType = (title: String,massage: String)
//CaseIterable能將case變成陣列
enum main:Int,CaseIterable{
    case a = 0,b,c,d,e
    /*
     相當於這個寫法
     case a
     case b
     case c
     case d
     case e
     */
    var value:mainType{
        switch self {
        case .a:
            return ("a","aa")
        case .b:
            return ("b","bb")
        case .c:
            return ("c","cc")
        case .d:
            return ("d","dd")
        case .e:
            return ("e","ee")
        }
    }
}

print(main.a.value.massage)
// aa

```
