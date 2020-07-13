---
layout: post
title: "Android - Kotlin - Options Menu"
date: 2020-07-09 07:20:00 +0530
categories: Android Kotlin
---

#### Preview

![Preview Image](/assets/andoid/options-menu-1.png)
![Preview Image](/assets/andoid/options-menu-2.png)


### app/res/menu/main_menu_options.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/menuItemInfo"
        android:title="Info"
        android:icon="@drawable/ic_info"
        />

    <item
        android:id="@+id/menuItemBug"
        android:title="Report a Bug"
        android:icon="@drawable/ic_bug"
        app:showAsAction="never"
        />
</menu>
```

### activity_main.xml

```xml
<androidx.constraintlayout.widget.ConstraintLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.menu_main_settings, menu)
         return true
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return when (item.itemId) {
            R.id.menuItemInfo -> {
                Toast.makeText(this, "Info Menu Tapped", Toast.LENGTH_SHORT).show()
                true
            }
            R.id.menuItemBug -> {
                Toast.makeText(this, "Info Bug Tapped", Toast.LENGTH_SHORT).show()
                true
            }
            else -> {
                super.onOptionsItemSelected(item)
            }
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```