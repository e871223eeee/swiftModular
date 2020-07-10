**將API流程模組化**

`API主程式`  
`新增InformationUpdate`
```swift
import Foundation
import UIKit

class InformationUpdate{
    static var shared = InformationUpdate()
    //@escaping 逃逸閉包 讓該func能在執行完後 閉包不會被清除
    func requestData<E: Encodable, D: Decodable>(_ method:HTTPMethod ,
                                                 _ apiURL:ApiPathConstants,
                                                 _ parameters:E ,
                                                 _ success: @escaping (D?)->()) {
        
        //轉json格式
        let jsonEncoder = JSONEncoder()
        let jsonData = try? jsonEncoder.encode(parameters)
        
        //設定URL
        let baseURL = NetworkConstants.baseUrl
        let url = URL(string: baseURL + apiURL.rawValue)!
        
        //設定URLRequest的url,method,body
        var request = URLRequest(url: url)
        request.httpMethod = method.rawValue
        
        if method == .get{
            //做url的處理
            //待補充
        }else{
            request.httpBody = jsonData
        }

        //打API
        let task = URLSession.shared.dataTask(with: request) { data, response, error in
            //如果沒通過就跳guard的func
            guard let data = data, error == nil else {
                print(error?.localizedDescription ?? "No data")
                return
            }
            
            //解碼
            let decoder = JSONDecoder()
            //按照傳入格式
            if let Data = try? decoder.decode(D.self, from: data)
            {
                //成功後將解碼完成的資料回傳 利用逃逸閉包 在viewController執行func
                //Data為資料
                success(Data)
            } else {
                print("error")
            }
            
            //解json 方便觀察資料用
            let responseJSON = try? JSONSerialization.jsonObject(with: data, options: [])
            if let responseJSON = responseJSON as? [String: Any] {
                print(responseJSON)
            }
        }
        task.resume()
    }
}
```

`URL及格式相關設定檔案`  
`新增NetworkConstants`
```swift
import Foundation

struct NetworkConstants {
    
    static let baseUrl = "https://"
    
    enum HttpHeaderField: String {
        case authentication = "Authorization"
        case contentType = "Content-Type"
        case acceptType = "Accept"
        case acceptEncoding = "Accept-Encoding"
    }
    
    enum ContentType: String {
        case json = "application/json"
        case xml = "application/xml"
        case x_www_form_urlencoded = "application/x-www-form-urlencoded"
    }
}

enum HTTPMethod: String {
    case options = "OPTIONS"
    case get     = "GET"
    case head    = "HEAD"
    case post    = "POST"
    case put     = "PUT"
    case patch   = "PATCH"
    case delete  = "DELETE"
    case trace   = "TRACE"
    case connect = "CONNECT"
}

enum RequestError: String {
    case unknownError
    case connectionError
    case authorizationError
    case invalidRequest
    case notFound
    case invalidResponse
    case serverError
    case serverUnavailable
    case internalError
}

enum ApiPathConstants: String {
    case api_1 = "your/api"
    case api_2 = "your/api-api"
}
```
`解碼及打api所需body格式`  
`新增apiCodableStruct`
```swift
import Foundation

//body格式
struct api_1_Parameters:Encodable {
    var value_1:Int = 5
    var value_2:Bool = true
    var value_3:String = "holle"
}

//接收後要解碼的格式
struct api_1_Response:Decodable {
    var CmdRsp:String
    var Code:Int
    var ErrorMsg:String
}

//body格式
struct api_2_Parameters:Encodable {
    var value_1:Int = 0
}

//接收後要解碼的格式
struct api_2_Response:Decodable {
    var CmdRsp:String
    var Code:Int
    var ErrorMsg:String
}
```
`實際使用`
```swift
//選擇要上傳的格式
var parameters:api_1_Parameters = api_1_Parameters()
        
//可以將值進行修改
parameters.value_1 = 1
        
//    打api
//    InformationUpdate.shared.requestData(你要的協定, 要打的URL, 上傳的body) { (解碼完的資料變數：解碼的格式) in
//        你要做的事
//    }
        
InformationUpdate.shared.requestData(.post, .api_1, parameters) { (data: api_1_Response!) in
  print(data.ErrorMsg)
}
```

`未完成 有新東西會持續加入`
