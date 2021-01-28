---
layout: post
title: "Android - Kotlin - Share Intent - text/plain"
date: 2020-07-18 07:20:00 +0530
categories: Android Kotlin
---

Showing share intent from a fragment

```kotlin
class AboutFragment : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val view = inflater.inflate(R.layout.fragment_about, container, false)
        setHasOptionsMenu(true)
        return view
    }

    override fun onCreateOptionsMenu(menu: Menu, inflater: MenuInflater) {
        super.onCreateOptionsMenu(menu, inflater)
        inflater.inflate(R.menu.overflow_about_menu, menu)
        // If we don't have intent which can share, hide menu.
        if (null == getShareIntent().resolveActivity(requireActivity().packageManager)) {
            menu.findItem(R.id.shareMenu).isVisible = false
        }
    }

    // Get Share Intent
    private fun getShareIntent() : Intent {
        return Intent(Intent.ACTION_SEND)
            .setType("text/plain")
            .putExtra(Intent.EXTRA_TEXT, "This is some sharable text")
    }

    // Show share intent
    private fun showShareIntent() {
        startActivity(getShareIntent())
    }

    // On tap of menu item, show share intent
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            R.id.shareMenu -> showShareIntent()
        }
        return super.onOptionsItemSelected(item)
    }
}
```