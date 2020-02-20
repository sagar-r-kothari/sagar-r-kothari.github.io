---
layout: post
title: "Swift - In App Purchase Receipt Validation"
date: 2020-02-04 07:20:00 +0530
categories: InAppPurchase
---

```swift
struct IAPReceiptValidator {
  func validateIAPReceipt() {
    guard 
      let receiptURL = Bundle.main.appStoreReceiptURL,
      let receiptData = try? Data(contentsOf: receiptURL)
      else { return }
    let receipt = receiptData.base64EncodedString()

    // Download password from appstoreconnect - your app - iap section - app secret
    let data = ["receipt-data": receipt, "password": "APPSTORE-IAP-SECRET"]
    guard
      JSONSerialization.isValidJSONObject(data)
      else { debugPrint("requestDictionary is not valid JSON"); return }
    do {
      let requestData = try JSONSerialization.data(withJSONObject: data)
      // In production, use https://buy.itunes.apple.com/verifyReceipt as the URL
      let validationURLString = "https://sandbox.itunes.apple.com/verifyReceipt"
      guard
        let validationURL = URL(string: validationURLString)
        else { debugPrint("the validation url could not be created, unlikely error"); return }
      let session = URLSession(configuration: URLSessionConfiguration.default)
      var request = URLRequest(url: validationURL)
      request.httpMethod = "POST"
      request.cachePolicy = URLRequest.CachePolicy.reloadIgnoringCacheData
      let task = session.uploadTask(with: request, from: requestData) { (data, response, error) in
        if let data = data , error == nil {
          do {
            let appReceiptJSON = try JSONSerialization.jsonObject(with: data)
            debugPrint("success. here is the json representation of the app receipt: \(appReceiptJSON)")
            // if you are using your server this will be a json representation of whatever your server provided
          } catch let error as NSError {
            debugPrint("json serialization failed with error: \(error)")
          }
        } else {
          debugPrint("the upload task returned an error: \(error?.localizedDescription ?? "")")
        }
      }
      task.resume()
    } catch let error as NSError {
      debugPrint("json serialization failed with error: \(error)")
    }
  }
}
```