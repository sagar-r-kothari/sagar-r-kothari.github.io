---
layout: post
title: "Android - Kotlin - FAB with options"
date: 2020-07-08 07:20:00 +0530
categories: Android Kotlin
---

#### Preview

![Preview Image](/assets/andoid/fab-with-option-1.png)
![Preview Image](/assets/andoid/fab-with-option-2.png)


### build.gradle (Module: app)

```rb
apply plugin: 'kotlin-kapt'
dependencies {
    ...
    // Material design
    implementation "com.google.android.material:material:1.1.0"
    ...
}
```

### activity_main.xml

We'll add five Fab buttons as indicated in screenshots above.

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

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fabBug"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:fabSize="mini"
        android:backgroundTint="@color/colorFabMini"
        android:src="@drawable/ic_bug"
        app:rippleColor="@color/colorPrimary"
        android:layout_margin="16dp"
        android:visibility="invisible"
        app:layout_constraintLeft_toLeftOf="@id/fab"
        app:layout_constraintRight_toRightOf="@id/fab"
        app:layout_constraintBottom_toTopOf="@id/fabInfo"/>

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fabInfo"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:fabSize="mini"
        android:backgroundTint="@color/colorFabMini"
        android:src="@drawable/ic_info"
        app:rippleColor="@color/colorPrimary"
        android:layout_margin="16dp"
        android:visibility="invisible"
        app:layout_constraintLeft_toLeftOf="@id/fab"
        app:layout_constraintRight_toRightOf="@id/fab"
        app:layout_constraintBottom_toTopOf="@id/fabFilter"/>

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fabFilter"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:fabSize="mini"
        android:backgroundTint="@color/colorFabMini"
        android:src="@drawable/ic_filter"
        app:rippleColor="@color/colorPrimary"
        android:layout_margin="16dp"
        android:visibility="invisible"
        app:layout_constraintLeft_toLeftOf="@id/fab"
        app:layout_constraintRight_toRightOf="@id/fab"
        app:layout_constraintBottom_toTopOf="@id/fabSort"/>

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fabSort"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:fabSize="mini"
        android:src="@drawable/ic_sort"
        android:backgroundTint="@color/colorFabMini"
        app:rippleColor="@color/colorPrimary"
        android:layout_margin="16dp"
        android:visibility="invisible"
        app:layout_constraintLeft_toLeftOf="@id/fab"
        app:layout_constraintRight_toRightOf="@id/fab"
        app:layout_constraintBottom_toTopOf="@id/fab"/>

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:src="@drawable/ic_add"
        app:rippleColor="@color/colorPrimary"
        android:layout_margin="16dp"
        app:layout_constraintBottom_toBottomOf="@id/listView"
        app:layout_constraintEnd_toEndOf="@id/listView"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    var rotate = false

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        fab.setOnClickListener {
            rotate = !rotate
            val rotationAngle =  if (rotate) 135f else 0f
            val alpha = if (rotate) 1f else 0f
            val visible = if (rotate) View.VISIBLE else View.INVISIBLE
            val color = ContextCompat.getColor(this, if (rotate) R.color.colorFabClose else R.color.colorAccent)
            fab.animate().setDuration(200).rotation(rotationAngle)
            fabBug.animate().setDuration(200).alpha(alpha).setUpdateListener {
                fabBug.visibility = visible
            }
            fabInfo.animate().setDuration(200).alpha(alpha).setUpdateListener {
                fabInfo.visibility = visible
            }
            fabSort.animate().setDuration(200).alpha(alpha).setUpdateListener {
                fabSort.visibility = visible
            }
            fabFilter.animate().setDuration(200).alpha(alpha).setUpdateListener {
                fabFilter.visibility = visible
            }
        }

        fabBug.setOnClickListener {
            Toast.makeText(this, "Bug Tapped", Toast.LENGTH_SHORT).show()
        }

        fabInfo.setOnClickListener {
            Toast.makeText(this, "Info Tapped", Toast.LENGTH_SHORT).show()
        }

        fabSort.setOnClickListener {
            Toast.makeText(this, "Sort Tapped", Toast.LENGTH_SHORT).show()
        }

        fabFilter.setOnClickListener {
            Toast.makeText(this, "Filter Tapped", Toast.LENGTH_SHORT).show()
        }
    }
}
```