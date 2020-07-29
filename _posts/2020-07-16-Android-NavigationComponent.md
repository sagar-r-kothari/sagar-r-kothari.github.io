---
layout: post
title: "Android - Kotlin - Navigation Component"
date: 2020-07-16 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```rb
dependencies {
    ...
    implementation 'androidx.navigation:navigation-fragment-ktx:2.3.0'
    implementation 'androidx.navigation:navigation-ui-ktx:2.3.0'
    ...
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

### Step 2. Update ***activity_main.xml***

```xml
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <!-- Following is important
    It adds a host fragment.
    It points to navigation graph.
    It also says that this is default host.
    Just copy & paste this. -->
    <fragment
        android:id="@+id/myNavHostFragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:navGraph="@navigation/navigation"
        app:defaultNavHost="true" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### Step 3. Add ***menu/main_overflow_menu.xml*** file.

- Right click on `res` folder.
- Add a menu `menu/main_overflow_menu.xml` file.
- Add one menu item `About`
- File should look as follows.
- Notice that menu-item-id is matching to `navigation/navigation.xml` fragment id for about fragment.

### Step 4. update ***MainFragment.kt*** as follows.


```kt
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        val view = inflater.inflate(R.layout.fragment_main, container, false)
        setHasOptionsMenu(true)
        return view
    }

    override fun onCreateOptionsMenu(menu: Menu, inflater: MenuInflater) {
        super.onCreateOptionsMenu(menu, inflater)
        inflater.inflate(R.menu.main_overflow_menu, menu)
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return NavigationUI.onNavDestinationSelected(item, requireView().findNavController())
                || super.onOptionsItemSelected(item)
    }
```

### Step 5. Provide Back / Up button support

- Open `MainActivity.kt` file.

```
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val navController = this.findNavController(R.id.myNavHostFragment)
        NavigationUI.setupActionBarWithNavController(this, navController)
    }

    override fun onSupportNavigateUp(): Boolean {
        val navController = this.findNavController(R.id.myNavHostFragment)
        return navController.navigateUp()
    }
```