---
layout: post
title: "Swift - Using UIImagePickerController"
date: 2020-02-24 07:20:00 +0530
categories: Swift
---

1. Open your View controller file.
2. Handle Event - e.g. `buttonTapped` in following example.
3. Add code snippet as follows

```swift
class YourViewController: UIViewController {

  func buttonTapped(_ sender: AnyObject?) {
    // 1. Create image picker controller
    let imagePicker = UIImagePickerController()
    // 2. Assign delegate
    imagePicker.delegate = self
    // 3. Set camera or other
    imagePicker.sourceType = .camera // .photoLibrary // .savedPhotosAlbum
    // 4. allow Editing only if required
    imagePicker.allowsEditing = true
    // present image picker.
    present(imagePicker, animated: true, completion: nil)
  }

}
```

4. Add delegate methods as follows.

```swift
extension YourViewController: UIImagePickerControllerDelegate {
    func imagePickerController(
      controller: UIImagePickerController, 
      didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey: Any]
    ) {
        controller.dismiss(animated: true, completion: nil)
        if let image = info[UIImagePickerController.InfoKey.editedImage] as? UIImage {
            // Do something when image is selected
        }
    }

    func imagePickerControllerDidCancel(controller: UIImagePickerController) {
        imagePicker.dismiss(animated: true, completion: nil)
    }
}
```