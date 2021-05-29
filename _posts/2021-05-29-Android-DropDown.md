---
layout: post
title: "Android - Show MaterialDropdown"
date: 2021-05-29 10:00:00 +0530
categories: Android Material Dropdown
---

![Preview Image](/assets/andoid/dropdown-1.png)
![Preview Image](/assets/andoid/dropdown-2.png)

```xml
<com.google.android.material.textfield.TextInputLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_marginBottom="10dp"
    android:layout_weight="1"
    style="@style/Widget.MaterialComponents.TextInputLayout.OutlinedBox.ExposedDropdownMenu"
    app:startIconDrawable="@drawable/ic_baseline_calendar_today_24">

    <com.google.android.material.textfield.MaterialAutoCompleteTextView
        android:id="@+id/expMonthFieldEditCard"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="@string/expiration_month"
        android:imeOptions="actionNext|actionPrevious"
        android:maxLines="1"
        android:inputType="none"
        android:textAppearance="@style/TextAppearance.AppCompat.Body1" />
</com.google.android.material.textfield.TextInputLayout>
```

Now, add following to your fragment / activity file.

```kotlin
val monthsAdapter = ArrayAdapter(
    requireContext(),
    android.R.layout.simple_spinner_item, months)
binding.expMonthFieldEditCard.setAdapter(monthsAdapter)
```