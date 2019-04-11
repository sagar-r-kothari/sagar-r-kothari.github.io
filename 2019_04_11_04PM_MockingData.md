### Mocking data with `Mocky` and [`randomuser`](https://randomuser.me/)

I personally recommend using [`Mocky`](https://www.mocky.io/) to Mock your HTTP responses to test your REST API.

Here is an example of sample web-service which I just created at `Mocky` which returns JSON.

Here is the piece of code, which you might be needing in iOS Application to run above mock service.

```swift
let string = "http://www.mocky.io/v2/5a1409093100002923b33fc0"
// create expectation
let task = URLSession.shared.dataTask(with: URL(string: string)!) { (data, response, error) in
  if error != nil && data != nil && data!.count > 0 {
    // no error & data received
  } else {
    // either error or no data.
  }
}
task.resume()
```