---
layout: post
title: "SwiftUI - FAB Action Button"
date: 2020-02-13 06:20:00 +0530
categories: SwiftUI
---

#### Preview

![Preview Image](/assets/SwiftUI-FabAction.png)

#### Code

```swift
import SwiftUI
struct ContentView: View {
    var body: some View {
        NavigationView {
            ZStack {
                List {
                    Text("Hello World")
                }
                VStack {
                    Spacer()
                    HStack {
                        Spacer()
                        Image(systemName: "plus")
                            .frame(width: 50, height: 50)
                            .background(Circle().fill(Color.accentColor))
                            .foregroundColor(.white)
                            .clipShape(Circle())
                            .shadow(color: .gray, radius: 3, x: 2, y: 2)
                            .padding()
                    }
                }
            }
        .navigationBarTitle("Some Title Here")
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```