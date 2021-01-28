---
layout: post
title: "Android - Change Root of Navigation Stack"
date: 2021-01-25 11:00:00 +0530
categories: Android Navigation
---

Let's say, after signing in, App shouldn't go back to login page or any other page.

Here is how you do it.

### Step 1. Open and edit navigation graph as follows.

![Preview Image](/assets/andoid/no-pop.png)

### Step 2. Navigation.xml should look like this.

```xml
<fragment
        android:id="@+id/loginFragment"
        android:name="com.sagar.test.LoginFragment"
        android:label="@string/app_name"
        tools:layout="@layout/fragment_login">
        <action
            android:id="@+id/action_loginFragment_to_dashboardFragment"
            app:destination="@id/dashboardFragment"


            app:launchSingleTop="true"
            app:popUpTo="@id/navigation"
            app:popUpToInclusive="true" 

            
            app:enterAnim="@anim/slide_in_right"
            app:exitAnim="@anim/slide_out_left"
            app:popEnterAnim="@anim/slide_in_left"
            app:popExitAnim="@anim/slide_out_right"/>
    </fragment>
```

### Step 3. Open DashboardFragment.kt file

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