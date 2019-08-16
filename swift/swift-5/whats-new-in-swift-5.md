---
description: Basics of Swift 5
---

# What's new in Swift 5?

#### Testing interger multiples

```swift
let someNumber = 10
if someNumber.isMultiple(of: 5) {
	print("It's multiple of 5")
} else {
	print("It's NOT multiple of 5")
}
```

```text
Output:
It's multiple of 5
```

#### Escaping Raw strings

```swift
var rawText = #"""
Hey! I can write anything "here"
without using double double quotes \#(someNumber)
"""#
```

```text
Output:
Hey! I can write anything "here"
without using double double quotes 10
```

#### isNumber to check a char value

```swift
var text = "SK10"
for char in text {
    print("Is '\(char)' number? \(char.isNumber)")
}
```

```text
Output:
Is 'S' number? false
Is 'K' number? false
Is '1' number? true
Is '0' number? true
```

Similarly, `isASCII`, `isLetter`, `isSymbol`, `isSymbol` etc. are available. Go ahead & give a try.

#### Use 'KeyValuePairs' instead 'DictionaryLiteral'

```swift
// Swift 4.2 Code
let data: DictionaryLiteral = ["name": "Sagar", "post": "iOS App Dev"]
print(data)

// Swift 5 code
let newData: KeyValuePairs = ["name": "Sagar", "post": "iOS App Dev"]
print(newData)
```

```text
Output:
["name": "Sagar", "post": "iOS App Dev"]
["name": "Sagar", "post": "iOS App Dev"]
```

#### `compactMapValues`

In following example, with `compactMapValues`, I was able to convert `[String: String]` to `[String: Int]`

```swift
let someData = ["avacado": "five", "banana": "10", "mango": "5", "lime": "30"]
let values = someData.compactMapValues(Int.init)
print("Values are \(values)")
```

```text
Output:
Values are ["mango": 5, "lime": 30, "banana": 10]
```

#### Future Enum cases

```swift
enum Vehicle {
    case car, bike, truck
}
let myVehicle: Vehicle = .car
switch myVehicle {
    case .car:
        print("Driving a car")
    case .bike:
        print("Riding a bike")
    @unknown default: 
        print("Driving a vehicle")
}
```

