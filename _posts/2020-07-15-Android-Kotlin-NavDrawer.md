---
layout: post
title: "Android - Kotlin - Navigation Drawer"
date: 2020-07-15 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```rb
dependencies {
    ...
    implementation 'com.google.android.material:material:1.1.0'
    ...
}
```

### Step 1: Add Layout files.

- `app/res/layout/content_main.xml` - which will have contents of main activity
- `app/res/layout/nav_header.xml` - which will contain navigation header

### Step 2: Update styles

- Open `app/res/styles.xml`
- Add following style within `<resources>` tag.

```xml
<style name="AppTheme.NoActionBar">
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
</style>
```

Final `styles.xml` should look like this.

```xml
<resources>
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="AppTheme.NoActionBar">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>

</resources>
```

### Step 3: Add a menu for Drawer Menu.

- Add file `app/res/menu/nav_menu.xml`

If `menu` folder doesn't exists, please create it under `app/res`

### Step 4: Remove ActionBar from MainActivity

- Open `app/manifests/AndroidManifest.xml`
- Add theme attribute to MainActivity as illustrated below.

```xml
<activity 
    android:name=".MainActivity"
    android:theme="@style/AppTheme.NoActionBar">
```

You can run this on Emulator / Device to make sure that ActionBar is now hidden.

### Step 5: Add DrawerLayout to MainActivity's layout file.

DrawerLayout will contain just two elements.
- Main container
- Drawer Menu

In Drawer Menu we can provide both
- header Layout
- Menu to show.

Here is how xml would look like of `activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/drawerLayout"
    android:fitsSystemWindows="true"
    tools:openDrawer="start">

        <include layout="@layout/content_main"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>

        <com.google.android.material.navigation.NavigationView
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:id="@+id/navHeader"
            android:layout_gravity="start"
            android:fitsSystemWindows="true"
            app:headerLayout="@layout/nav_header"
            app:menu="@menu/nav_menu"
            />

</androidx.drawerlayout.widget.DrawerLayout>
```

### Step 6: Design for DrawerMenu - header

- I'll add an imageview and two text views for header of drawer menu. 
- Something like as follows.
- `app/res/layout/nav_header.xml`

![Preview Image](/assets/andoid/drawer-header.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:layout_marginTop="10dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@mipmap/ic_launcher" />

    <TextView
        android:id="@+id/titleText"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:layout_marginTop="10dp"
        android:layout_marginEnd="10dp"
        android:text="Sagar Kothari"
        android:textAppearance="@style/TextAppearance.AppCompat.Medium"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/imageView2" />

    <TextView
        android:id="@+id/subtitleText"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:layout_marginTop="10dp"
        android:layout_marginEnd="10dp"
        android:text="iOS and Android App Developer"
        android:textAppearance="@style/TextAppearance.AppCompat.Small"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/titleText" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

### Step 7: Menu for DrawerMenu

- I've added few dummy menu items. Please modify it as per your need.
- `app/res/menu/nav_menu.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
<group>
    <item
        android:id="@+id/navFriends"
        android:icon="@drawable/icon_user_profile"
        android:title="Friends" />
    <item
        android:id="@+id/navMessages"
        android:icon="@drawable/icon_messages"
        android:title="Messages"/>
    <item
        android:id="@+id/navWallet"
        android:icon="@drawable/icon_wallet"
        android:title="Wallet" />
</group>
    <item android:title="Account Settings">
        <menu>
            <item
                android:id="@+id/navEdit"
                android:icon="@drawable/icon_edit"
                android:title="Edit Profile" />
            <item
                android:id="@+id/navLogout"
                android:icon="@drawable/icon_logout"
                android:title="Log out" />
        </menu>
    </item>
</menu>
```

### Step 8: Finish design for Main Content

- I've added few dummy elements. Please modify it as per your need.
- Make sure you add AppBarLayout 
- `app/res/layout/content_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <com.google.android.material.appbar.AppBarLayout
            android:layout_height="wrap_content"
            android:layout_width="match_parent"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">

        <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="?attr/colorPrimary"
                app:popupTheme="@style/ThemeOverlay.AppCompat.Light">
            <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:orientation="horizontal"
                    android:gravity="center_vertical">
                <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Navigation Drawer"
                        android:textColor="#FFFFFF"
                        style="@style/TextAppearance.AppCompat.Widget.ActionBar.Title"/>
            </LinearLayout>
        </androidx.appcompat.widget.Toolbar>

    </com.google.android.material.appbar.AppBarLayout>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello Drawer Menu"
        />

</LinearLayout>
```

### Step 9: MainActivity.kt

Access Drawer Menu & add listners

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Toolbar is support bar
        setSupportActionBar(toolbar)

        // Set Toggle - open-close menu
        val toggle = ActionBarDrawerToggle(this, drawerLayout, toolbar, 0, 0)
        drawerLayout.addDrawerListener(toggle)
        toggle.syncState()

        // On click of menu
        navHeader.setNavigationItemSelectedListener {
            when (it.itemId) {
                R.id.navEdit -> {
                    Toast.makeText(this,"Edit profile clicked", Toast.LENGTH_LONG).show()
                }
            }
            return@setNavigationItemSelectedListener true
        }
    }
}
```