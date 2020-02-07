---
layout: post
title: "SwiftUI - UIImagePickerView"
date: 2020-02-07 04:20:00 +0530
categories: SwiftUI
---

Following code snippet illustrates how to use image picker.

```swift
struct SomeViewOfYourProject: View {
    @State private var showImagePicker: Bool = false
    @State private var image: Image? = nil

    var body: some View {
        VStack {
            Text("Hello World")
            assignPhotoButton
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
    @Binding var image: Image?

    func updateUIViewController(
        _ uiViewController: UIImagePickerController,
        context: UIViewControllerRepresentableContext<ImagePicker>) {
    }

    func makeCoordinator() -> ImagePickerCordinator {
        ImagePickerCordinator(isShown: $isShown, image: $image)
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

class ImagePickerCordinator : NSObject, UINavigationControllerDelegate, UIImagePickerControllerDelegate{
    @Binding var isShown: Bool
    @Binding var image: Image?

    init(isShown : Binding<Bool>, image: Binding<Image?>) {
        _isShown = isShown
        _image   = image
    }

    func imagePickerController(
        _ picker: UIImagePickerController,
        didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        let uiImage = info[UIImagePickerController.InfoKey.editedImage] as! UIImage
        image = Image(uiImage: uiImage)
        isShown = false
    }

    func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
        isShown = false
    }
}

struct PhotoCaptureView: View {
    @Binding var showImagePicker: Bool
    @Binding var image: Image?

    var body: some View {
        ImagePicker(isShown: $showImagePicker, image: $image)
    }
}

struct PhotoCaptureView_Previews: PreviewProvider {
    static var previews: some View {
        PhotoCaptureView(showImagePicker: .constant(false), image: .constant(Image("")))
    }
}
```