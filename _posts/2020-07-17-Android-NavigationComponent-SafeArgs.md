---
layout: post
title: "Android - Kotlin - Navigation Component - SafeArgs"
date: 2020-07-17 07:20:00 +0530
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
    dependencies {
        ...
        implementation 'androidx.navigation:navigation-fragment-ktx:2.3.0'
        implementation 'androidx.navigation:navigation-ui-ktx:2.3.0'
        ...
    }
    # AND THIS
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    # AND THIS
    kotlinOptions {
        jvmTarget = JavaVersion.VERSION_1_8.toString()
    }
```

### Step 1. Add fragments

- Right click on package. 
- Add new fragment.
- Blank fragment
- Add `MainFragment.kt` & `DetailFragment.kt`
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
        android:label="Main Fragment"
        tools:layout="@layout/fragment_main">
        <action
            android:id="@+id/action_mainFragment_to_detailFragment"
            app:destination="@id/detailFragment">
            <argument
                android:name="textContent"
                android:defaultValue="This is a default value to be passed to Add new entry fragment." />
        </action>
    </fragment>
    <fragment
        android:id="@+id/detailFragment"
        android:name="com.sagar_r_kothari.navdemo.DetailFragment"
        android:label="Detail Fragment"
        tools:layout="@layout/fragment_detail">
        <argument
            android:name="textContent"
            app:argType="string" />
    </fragment>
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

### Step 3. Update ***fragment_main.xml***

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainFragment">

    <TextView
        android:id="@+id/textView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="16dp"
        android:text="@string/hello_main_fragment"
        android:textAlignment="center"
        android:textAppearance="@style/TextAppearance.AppCompat.Large"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/addEntryButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="16dp"
        android:text="@string/add_entry"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### Step 3. update ***MainFragment.kt*** as follows.

```kotlin
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val view = inflater.inflate(R.layout.fragment_main, container, false)
        view.addEntryButton.setOnClickListener {
            val testText = "This is my test text"
            // Access action & pass value
            val action = MainFragmentDirections.actionMainFragmentToDetailFragment(testText)
            requireView().findNavController().navigate(action)
        }
        return view
    }
```

### Step 4. Update ***fragment_detail.xml***

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".DetailFragment">

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="@string/hello_blank_fragment" />

</FrameLayout>
```

### Step 5. Update ***DetailFragment.kt***

```kotlin
class DetailFragment : Fragment() {
    val args: AddNewEntryFragmentArgs by navArgs() // <<-- 1. arguments

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.fragment_add_new_entry, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        textView.text = args.textContent // <<-- 2. read from arguments
    }
}
```