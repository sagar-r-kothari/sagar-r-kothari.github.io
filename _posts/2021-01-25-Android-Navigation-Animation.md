---
layout: post
title: "Android - Animate Fragment on Navigation Graph"
date: 2021-01-25 07:00:00 +0530
categories: Android Animate
---

#### Create following files under `app/src/main/res/anim/` directory

##### `slide_in_left.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:toYDelta="0%"
        android:toXDelta="0%"
        android:fromYDelta="0%"
        android:fromXDelta="-100%"
        android:duration="@android:integer/config_mediumAnimTime"
        />
</set>
```

##### `slide_in_right.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:toYDelta="0%"
        android:toXDelta="0%"
        android:fromYDelta="0%"
        android:fromXDelta="100%"
        android:duration="@android:integer/config_mediumAnimTime"
        />
</set>
```

##### `slide_out_left.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:toYDelta="0%"
        android:toXDelta="-100%"
        android:fromYDelta="0%"
        android:fromXDelta="0%"
        android:duration="@android:integer/config_mediumAnimTime"
        />
</set>
```

##### `slide_out_right.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:toYDelta="0%"
        android:toXDelta="100%"
        android:fromYDelta="0%"
        android:fromXDelta="0%"
        android:duration="@android:integer/config_mediumAnimTime"
        />
</set>
```

#### Apply animation

- Open `navigation.xml` unde `app/src/main/res/navigation`
- Select the fragment-navigation-action, apply animation using IDE or xml should be as follows.

In this example, it's from home fragment to about fragment.

```xml
<fragment
        android:id="@+id/homeFragment"
        android:name="com.snanydevicess.drjitendraarya.HomeFragment"
        android:label="@string/dr_jitendra_arya"
        tools:layout="@layout/fragment_home" >
        <action
            android:id="@+id/action_homeFragment_to_aboutFragment"
            app:destination="@id/aboutFragment"
            app:enterAnim="@anim/slide_in_right"
            app:exitAnim="@anim/slide_out_left"
            app:popEnterAnim="@anim/slide_in_left"
            app:popExitAnim="@anim/slide_out_right" />
```