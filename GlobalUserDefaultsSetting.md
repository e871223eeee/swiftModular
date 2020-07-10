**將UserDefaults拉出來進行統一管理**  
`以bool,int,string為例`  
`開新檔案GlobalUserDefaultsSetting`

```swift
import UIKit

class GlobalUserDefaultsSetting {
    static let shared = GlobalUserDefaultsSetting()
    private let userPreferences: UserDefaults
    
    //初始化
    private init() { userPreferences = UserDefaults.standard }
    
    //利用enum的方式來選取要使用的key
    enum UserPreference: String {
        case YourBool
        case YourInt
        case YourString
    }
    
    //使用 rawValve 方法取得 enum 的文字
    
    //預設值可給可不給
    var YourBool:Bool{
        get{return userPreferences.bool(forKey: UserPreference.YourBool.rawValue)}
        set{userPreferences.set(newValue, forKey: UserPreference.YourBool.rawValue)}
    }
   
    //預設值可給可不給
    var YourInt: Int {
        get{ return userPreferences.integer(forKey: UserPreference.YourInt.rawValue) }
        set{ userPreferences.set(newValue, forKey: UserPreference.YourInt.rawValue) }
    }
    
    //字串要給預設值
    var YourString: String {
        get{ return userPreferences.string(forKey: UserPreference.YourString.rawValue) ?? ""}
        set{ userPreferences.set(newValue, forKey: UserPreference.YourString.rawValue) }
    }
    
    //刪除資料
    func removeData(_ inputKey:UserPreference){
        userPreferences.removeObject(forKey: inputKey.rawValue)
    }
}
```

`使用方式`  
`給值`
```swift
  GlobalUserDefaultsSetting.shared.YourBool = true
  GlobalUserDefaultsSetting.shared.YourInt = 0
  GlobalUserDefaultsSetting.shared.YourString = ""
```

`取值 以print為例`
```swift
  print(GlobalUserDefaultsSetting.shared.YourBool)
  print(GlobalUserDefaultsSetting.shared.YourInt)
  print(GlobalUserDefaultsSetting.shared.YourString)
```

`刪除資料`
```swift
GlobalUserDefaultsSetting.shared.removeData(.YourBool)
GlobalUserDefaultsSetting.shared.removeData(.YourInt)
GlobalUserDefaultsSetting.shared.removeData(.YourString)
```
