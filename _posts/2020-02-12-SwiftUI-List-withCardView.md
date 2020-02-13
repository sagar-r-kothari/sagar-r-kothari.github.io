---
layout: post
title: "SwiftUI - List view with Elevated Cards"
date: 2020-02-12 06:20:00 +0530
categories: SwiftUI
---

#### Preview

![Preview Image](/assets/SwiftUI-ListCardView.png)

#### Data Structure

```swift
struct ItemData: Identifiable {
    var id: Int
    var title: String
    var status: String
    var address: String
    var date: String
}
```

#### View for Single Item / Row

```swift
struct ItemView: View {
    var data: ItemData
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
                        Text(data.title)
                            .font(.callout)
                            .lineLimit(1)
                        Spacer()
                        Text(data.status)
                            .font(.caption)
                            .lineLimit(1)
                    }
                    HStack {
                        Text(data.address)
                            .font(.callout)
                            .lineLimit(1)
                        Spacer()
                        Text(data.date)
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
        }
    }
}
```

#### List view 

```swift
struct MyListView: View {
    var items: [ItemData]
    var body: some View {
        List(items) { item in
            ContentView(data: item)
        }.onAppear {
            UITableView.appearance().tableFooterView = UIView()
            UITableView.appearance().separatorStyle = .none
        }
    }
}
```

#### Dummy data for List View

```swift
extension MyListView {
    static var sharedItems = [
        ItemData(id: 1, title: "Lenses", status: "Ordered", address: "Hyd, Telangana", date: "25 Aug"),
        ItemData(id: 2, title: "Watch", status: "In Process", address: "Pune, Maharashtra", date: "27 Aug"),
        ItemData(id: 3, title: "Bag", status: "Out for delivery", address: "Ahmedabad, Gujarat", date: "28 Aug"),
        ItemData(id: 4, title: "Shoes", status: "Ready", address: "Vizag", date: "21 Aug")
    ]
}

struct MyListView_Previews: PreviewProvider {
    static var previews: some View {
        return MyListView(items: MyListView.sharedItems)
    }
}
```