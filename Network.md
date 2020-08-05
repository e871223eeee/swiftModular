**Network單例**

`新創Network.swift`  


```swift
import Foundation
import UIKit
import Network

class Network {
    static let shared = Network()
    
    let monitor = NWPathMonitor()
    
    //目前網路狀態
    var myNetworkStatus:NetworkStatus = .not

    
     // 啟用網路偵測
    func start(){
        monitor.pathUpdateHandler = { path in
            // wifi
            if path.usesInterfaceType(.wifi){
                self.myNetworkStatus = .wifi
                print("wifi")
            // 行動網路
            }else if path.usesInterfaceType(.cellular) {
                self.myNetworkStatus = .cellular
                print("cellular")
            // 以太網路
            }else if path.usesInterfaceType(.wiredEthernet) {
                self.myNetworkStatus = .wiredEthernet
                print("wiredEthernet")
            // local host
            }else if path.usesInterfaceType(.loopback) {
                self.myNetworkStatus = .loopback
                print("loopback")
            // 虛擬、未知
            }else if path.usesInterfaceType(.other){
                self.myNetworkStatus = .other
                print("other")
            // 無網路連線
            }else{
                self.myNetworkStatus = .not
                print("not")
            }
        }
        
        monitor.start(queue: DispatchQueue.global())
    }
    
    //目前網路狀態
    func status() -> NetworkStatus {
        return myNetworkStatus
    }
    
    enum NetworkStatus{
        // wifi
        case wifi
        // 行動網路
        case cellular
        // 以太網路
        case wiredEthernet
        // local host
        case loopback
        // 虛擬、未知
        case other
        // 無網路連線
        case not
    }
}

```
`使用`
```swift
  //開啟網路偵測
  Network.shared.start()
  
  //目前狀態
  print(Network.shared.status())
```
