---
layout: post
title: "Swift - Form-Data Post Request"
date: 2020-02-20 07:20:00 +0530
categories: Swift
---

Let's say, you've to send following request from iOS app.

```curl
curl -X POST \
  https://some.server.com/api/endpoint \
  -H 'cache-control: no-cache' \
  -F 'signup[email]=user@mail.com' \
  -F 'signup[password]=password' \
  -F product_id=productidhere \
  -F transaction_id=sometransactionid
```

Here is the code to achieve it.

```swift
struct SignUpData {
  let userEmail: String
  let passData: String
  let productId: String
  let transactionId: String
  var parameters: [String: Any] { [
    "signup[email]": userEmail,
    "signup[password]": passData,
    "product_id": productId,
    "transaction_id": transactionId,
    "receipt": receipt
    ]
  }
}
```

```swift
struct SignUpService {
  func signup(
    with data: SignUpData,
    handler: @escaping (Swift.Result<Void, Error>) -> Void
  ) {
    // 1. create URL
    guard
      let url = URL(string: "https://some.server.com/api/endpoint")
      else { return }
    
    // 2. create request
    var request = URLRequest(url: url)

    // 3. set required headers
    request.setValue("no-cache", forHTTPHeaderField: "cache-control")

    // 4. set POST method
    request.httpMethod = "POST"

    // 5. get percent encoded data of parameters 
    // - for `percentEncoded()` see code below this block.
    let body = data.parameters.percentEncoded()
    request.httpBody = body

    // 6. start task & handle response
    URLSession.shared.dataTask(with: request) { (data, response, error) in
      guard let data = data, error == nil else {
        // handle error here or no data here
      }
      if let httpStatus = response as? HTTPURLResponse, httpStatus.statusCode != 200 {
        // do something if statuscode is not 200
      } else {
        // handle successful response here
      }
    }.resume()
  }
}
```

Supporting percent Encoding with following code block

```swift
extension Dictionary {
  func percentEncoded() -> Data? {
    return map { key, value in
      let escapedKey = "\(key)".addingPercentEncoding(withAllowedCharacters: .urlQueryValueAllowed) ?? ""
      let escapedValue = "\(value)".addingPercentEncoding(withAllowedCharacters: .urlQueryValueAllowed) ?? ""
      return escapedKey + "=" + escapedValue
    }
    .joined(separator: "&")
    .data(using: .utf8)
  }
}

extension CharacterSet {
  static let urlQueryValueAllowed: CharacterSet = {
    let generalDelimitersToEncode = ":#[]@" // does not include "?" or "/" due to RFC 3986 - Section 3.4
    let subDelimitersToEncode = "!$&'()*+,;="

    var allowed = CharacterSet.urlQueryAllowed
    allowed.remove(charactersIn: "\(generalDelimitersToEncode)\(subDelimitersToEncode)")
    return allowed
  }()
}
```