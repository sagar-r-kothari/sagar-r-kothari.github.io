---
layout: post
title: "Swift UIElevated Card View"
date: 2020-02-12 05:20:00 +0530
categories: SwiftUI
---

![Preview Image](/assets/SwiftUI-CardView.png)

```swift
struct ContentView: View {
    var title: String
    var status: String
    var address: String
    var date: String

    var body: some View {
        HStack {
            HStack {
                Image("blaa")
                    .frame(width: 50, height: 50)
                    .background(Circle().fill(Color.gray))
                    .clipShape(Circle())
                    .padding(.trailing, 10)
                VStack(spacing: 5) {
                    HStack {
                        Text(title)
                            .font(.callout)
                            .lineLimit(1)
                        Spacer()
                        Text(status)
                            .font(.caption)
                            .lineLimit(1)
                    }
                    HStack {
                        Text(address)
                            .font(.callout)
                            .lineLimit(1)
                        Spacer()
                        Text(date)
                            .font(.caption)
                            .lineLimit(1)
                    }
                }
                Image(systemName: "info")
                    .frame(width: 25, height: 25)
                    .background(Circle().fill(Color.gray))
                    .padding(.leading, 10)
            }
            .padding()
            .background(Rectangle().fill(Color.white))
            .cornerRadius(10)
            .shadow(color: .gray, radius: 3, x: 2, y: 2)
        }.padding()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView(
            title: "Sagar",
            status: "In Progress",
            address: "Hyderabad, India",
            date: "Aug 23")
    }
}
```