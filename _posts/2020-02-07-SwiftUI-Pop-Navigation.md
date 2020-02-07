---
layout: post
title: "SwiftUI - Pop a view from Navigation"
date: 2020-02-07 04:20:00 +0530
categories: SwiftUI Navigation
---

```swift
import SwiftUI

struct MyListView: View {
    var body: some View {
        NavigationView {
            List {
                // Some data here
            }
            .navigationBarItems(trailing: addButton)
        }
    }

    var addButton: some View {
        NavigationLink(destination: AddView()) {
            Image(systemName: "plus")
        }
    }
}

struct AddView: View {
    @Environment(\.presentationMode) var presentationMode: Binding<PresentationMode>

    var body: some View {
        Form {
            // Some UI Elements here
        }
        .navigationBarTitle("Add Screen")
        .navigationBarItems(trailing: saveButton)
    }

    var saveButton: some View {
        Button(action: {
            self.presentationMode.wrappedValue.dismiss()
        }, label: {
            Text("Save")
        })
    }
}
```