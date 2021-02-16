---
layout: post
title: "Android - Change Root of Navigation Stack"
date: 2021-01-25 11:00:00 +0530
categories: Android Navigation
---

Let's say, after signing in, App shouldn't go back to login page or any other page.

Here is how you do it.

### Open `DashboardFragment.kt` file

Basically, with following, we are setting Dashboard Fragment as a root fragment of Nav Controller.
This means that even if you landed on Dashboard after log-in, pressing back button won't go back to log-in page.

```kotlin
class DashboardFragment : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Code to replace root fragment
        val activity = requireContext() as MainActivity
        val appBarConfiguration = AppBarConfiguration
            .Builder(R.id.dashboardFragment)
            .build()
        setupActionBarWithNavController(activity, findNavController(), appBarConfiguration)
        // -------
        return inflater.inflate(R.layout.fragment_dashboard, container, false)
    }
}
```