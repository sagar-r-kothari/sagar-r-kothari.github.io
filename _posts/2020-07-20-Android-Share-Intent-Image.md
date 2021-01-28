---
layout: post
title: "Android - Kotlin - Share Intent - ImageView's Image"
date: 2020-07-20 07:20:00 +0530
categories: Android Kotlin
---

```kotlin
private fun showShareIntent() {
    // Step 1: Create Share itent
    val intent = Intent(Intent.ACTION_SEND).setType("image/*")

    // Step 2: Get Bitmap from your imageView
    val bitmap = imageView.drawable.toBitmap() // your imageView here.

    // Step 3: Compress image
    val bytes = ByteArrayOutputStream()
    bitmap.compress(Bitmap.CompressFormat.JPEG, 100, bytes)

    // Step 4: Save image & get path of it
    val path = MediaStore.Images.Media.insertImage(requireContext().contentResolver, bitmap, "tempimage", null)

    // Step 5: Get URI of saved image
    val uri = Uri.parse(path)

    // Step 6: Put Uri as extra to share intent
    intent.putExtra(Intent.EXTRA_STREAM, uri)

    // Step 7: Start/Launch Share intent
    startActivity(intent)
}
```