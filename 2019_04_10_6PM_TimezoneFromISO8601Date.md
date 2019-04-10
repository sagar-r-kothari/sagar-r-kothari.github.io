## How to extract timezone from ISO8601 Date?

Let's say, you've a date ISO8601 date.

```
2019-04-10T10:30:00+5:30
```

For date above, Timezone is `Asia/Kolkatta`

```
extension TimeZone {
    init?(iso8601: String) {
        let tz = iso8601.dropFirst(19) // remove yyyy-MM-ddTHH:mm:ss part
        if tz == "Z" {
            self.init(secondsFromGMT: 0)
        } else if tz.count == 3 { // assume +/-HH
            if let hour = Int(tz) {
                self.init(secondsFromGMT: hour * 3600)
                return
            }
        } else if tz.count == 5 { // assume +/-HHMM
            if let hour = Int(tz.dropLast(2)), let min = Int(tz.dropFirst(3)) {
                self.init(secondsFromGMT: (hour * 60 + min) * 60)
                return
            }
        } else if tz.count == 6 { // assime +/-HH:MM
            let parts = tz.components(separatedBy: ":")
            if parts.count == 2 {
                if let hour = Int(parts[0]), let min = Int(parts[1]) {
                    self.init(secondsFromGMT: (hour * 60 + min) * 60)
                    return
                }
            }
        }
        return nil
    }
}

```

with above extension, we can easily achieve extraction of timezone from ISO8601 date.

```
let dateText = "2019-04-10T10:30:00+5:30"
let timeZone = TimeZone(iso8601: dateText)
```