---
layout: post
title: "Android - Hide Show ActionBar Menu Item"
date: 2021-02-13 08:00:00 +0530
categories: Android
---

Let's say you've a menu file as follows.

```xml
<!-- some_menu.xml -->
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/dashboard_menu_search_option"
        android:icon="@drawable/ic_baseline_search_24"
        android:title="@string/dashboard_menu_search_option"
        android:visible="true"
        app:showAsAction="always" />
</menu>
```

Following would be code in your fragment.

```kotlin
class MyFragment : Fragment() {
    private var _binding: FragmentMyBinding? = null
    private val binding get() = _binding!!

    // variable to hold reference of menu
    private var actionBarMenu: Menu? = null

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // This is important. Without this, menu won't appear.
        setHasOptionsMenu(true)
        _binding = FragmentMyBinding.inflate(inflater, container, false)
        setupView()
        return binding.root
    }

    override fun onCreateOptionsMenu(menu: Menu, inflater: MenuInflater) {
        inflater.inflate(R.menu.some_menu, menu)
        actionBarMenu = menu
        super.onCreateOptionsMenu(menu, inflater)
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return when (item.itemId) {
            R.id.dashboard_menu_search_option -> {
                item.isVisible = false // we are hiding menu
                true
            }
            else -> {
                super.onOptionsItemSelected(item)
            }
        }
    }

    private fun setupView() {
        binding.someButton.setOnClickListener {
            // we are showing menu on click of a button
            actionBarMenu?.findItem(R.id.dashboard_menu_search_option)?.isVisible = true
        }
    }
}
```