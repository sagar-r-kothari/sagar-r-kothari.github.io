---
layout: post
title: "SwiftUI - UIImagePickerView"
date: 2020-02-07 05:20:00 +0530
categories: SwiftUI
---

Following code snippet illustrates how to use image picker.

```swift
struct SomeViewOfYourProject: View {
    @State private var showImagePicker: Bool = false
    @State private var uiImage: UIImage? = nil

    var image: Image? {
        guard let uiImage = uiImage else { return nil }
        return Image(uiImage: uiImage)
    }

    var body: some View {
        VStack {
            Text("Hello World")
            assignPhotoButton
        }.sheet(isPresented: $showImagePicker) {
            PhotoCaptureView(showImagePicker: self.$showImagePicker, image: self.$uiImage)
        }
    }

    var assignPhotoButton: some View {
        VStack {
            if (image != nil) {
                image!
                    .resizable()
                    .scaledToFit()
                    .frame(width: 100, height: 100)
            } else {
                Image(systemName: "camera")
                    .frame(width: 100, height: 100)
                Text("Assign Photo")
            }
        }.onTapGesture {
            self.showImagePicker = true
        }
    }
}
```

Following code snippet, just copy and paste it into a file inside project.

```swift
struct ImagePicker: UIViewControllerRepresentable {
    @Binding var isShown: Bool
    @Binding var uiImage: UIImage?

    func updateUIViewController(
        _ uiViewController: UIImagePickerController,
        context: UIViewControllerRepresentableContext<ImagePicker>) {
    }

    func makeCoordinator() -> ImagePickerCordinator {
        ImagePickerCordinator(isShown: $isShown, image: $uiImage)
    }

    func makeUIViewController(
        context: UIViewControllerRepresentableContext<ImagePicker>
    ) -> UIImagePickerController {
        let picker = UIImagePickerController()
        picker.delegate = context.coordinator
        picker.allowsEditing = true
        return picker
    }
}

class ImagePickerCordinator : NSObject, UINavigationControllerDelegate, UIImagePickerControllerDelegate {
    @Binding var isShown: Bool
    @Binding var image: UIImage?

    init(isShown : Binding<Bool>, image: Binding<UIImage?>) {
        _isShown = isShown
        _image   = image
    }

    func imagePickerController(
        _ picker: UIImagePickerController,
        didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        image = info[UIImagePickerController.InfoKey.editedImage] as? UIImage
        isShown = false
    }

    func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
        isShown = false
    }
}

struct PhotoCaptureView: View {
    @Binding var showImagePicker: Bool
    @Binding var image: UIImage?

    var body: some View {
        ImagePicker(isShown: $showImagePicker, uiImage: $image)
    }
}
```