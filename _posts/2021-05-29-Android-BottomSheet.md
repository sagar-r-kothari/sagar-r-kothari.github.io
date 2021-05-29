---
layout: post
title: "Android - Full screen BottonSheetDialog"
date: 2021-05-29 11:00:00 +0530
categories: Android BottonSheetDialog
---

```kotlin
import android.app.Dialog
import android.content.res.Resources
import android.os.Bundle
import android.view.ViewGroup
import android.widget.FrameLayout
import com.google.android.material.bottomsheet.BottomSheetBehavior
import com.google.android.material.bottomsheet.BottomSheetDialog
import com.google.android.material.bottomsheet.BottomSheetDialogFragment

open class SKBottomSheetDialogFragment: BottomSheetDialogFragment() {
    override fun onStart() {
        super.onStart()
        val sheetContainer = requireView().parent as? ViewGroup ?: return
        sheetContainer.layoutParams.height = ViewGroup.LayoutParams.MATCH_PARENT
    }

    override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
        val dialog = super.onCreateDialog(savedInstanceState)
        dialog.setOnShowListener { dialogInterface ->
            (dialogInterface as? BottomSheetDialog)?.let { bottomSheetDialog ->
                (bottomSheetDialog.findViewById(R.id.design_bottom_sheet) as? FrameLayout)?.let { frameLayout ->
                    BottomSheetBehavior.from(frameLayout).peekHeight = Resources.getSystem().displayMetrics.heightPixels
                    BottomSheetBehavior.from(frameLayout).state = BottomSheetBehavior.STATE_EXPANDED
                }
            }
        }
        return dialog
    }
}
```

Now inherit it in your fragment which you want to show.

```kotlin
class EditCardFragment : SKBottomSheetDialogFragment() {
  // ....
  // your code goes here
  // ....
}
```

Now, show dialog on click of a button / or any event you wish.

```kotlin
binding.editButtonRealCardDetails.setOnClickListener {
    val fragment = EditCardFragment()
    fragment.show(parentFragmentManager, "edit_card")
}
```