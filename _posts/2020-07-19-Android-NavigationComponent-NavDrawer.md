---
layout: post
title: "Android - Kotlin - Navigation drawer with navigation component"
date: 2020-07-19 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Project leve)

```rb
dependencies {
    ...
    classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.3.0"
    ...
}
```


### build.gradle (Module: app)

```rb
    # MAKE SURE YOU DON'T MISS THIS
    apply plugin: "androidx.navigation.safeargs.kotlin"
    # AND THIS
android {
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    # AND THIS
    kotlinOptions {
        jvmTarget = JavaVersion.VERSION_1_8.toString()
    }
    # THIS IS MUST
    dataBinding {
        enabled = true
    }
}
dependencies {
    # ADD below dependencies
    # DON'T REMOVE existing ones
    # Navigation Fragments
    implementation 'androidx.navigation:navigation-fragment-ktx:2.3.0'

    # Navigation UI
    implementation 'androidx.navigation:navigation-ui-ktx:2.3.0'

    # Material components
    implementation 'com.google.android.material:material:1.1.0'
}
```

### Step 1. Add fragments

- Right click on package. 
- Add new fragment.
- Blank fragment
- Add `MainFragment.kt` & `AboutFragment.kt`
- It would also create respective resource files

### Step 2. Add navigation Graph.

- Right click on `res` folder.
- Add `navigation/navigation.xml` file.
- Add both fragments created.
- After adding those, it should look as follows.

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/navigation"
    app:startDestination="@id/mainFragment">

    <fragment
        android:id="@+id/mainFragment"
        android:name="com.sagar_r_kothari.navdemo.MainFragment"
        android:label="fragment_main"
        tools:layout="@layout/fragment_main" />
    <fragment
        android:id="@+id/aboutFragment"
        android:name="com.sagar_r_kothari.navdemo.AboutFragment"
        android:label="fragment_about"
        tools:layout="@layout/fragment_about" />
</navigation>
```

### Step 3. Add Menu for Drawer.

- Right click on `res` folder.
- Add `res/menu/navdrawer_menu.xml`

It should have contents as follows.

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/aboutFragment"
        android:title="@string/menu_about" />
</menu>
```

### Step 4. Update ***activity_main.xml***

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <androidx.drawerlayout.widget.DrawerLayout
        android:id="@+id/drawerLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <fragment
                android:id="@+id/myNavHostFragment"
                android:name="androidx.navigation.fragment.NavHostFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                app:navGraph="@navigation/navigation"
                app:defaultNavHost="true" />

        </LinearLayout>

        <com.google.android.material.navigation.NavigationView
            android:id="@+id/navView"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            app:menu="@menu/navdrawer_menu" />

    </androidx.drawerlayout.widget.DrawerLayout>
</layout>
```

### Step 5. Update ***MainActivity.kt*** code as follows.

```kt
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val navController = this.findNavController(R.id.myNavHostFragment)
        // Set up action bar
        NavigationUI.setupActionBarWithNavController(this, navController, drawerLayout)
        // Setup AppBar
        appBarConfiguration = AppBarConfiguration(navController.graph, drawerLayout)
        // Connect Navigation view & navigation Controller
        NavigationUI.setupWithNavController(navView, navController)
    }

    override fun onSupportNavigateUp(): Boolean {
        val navController = this.findNavController(R.id.myNavHostFragment)
        return NavigationUI.navigateUp(navController, drawerLayout)
    }
}
```