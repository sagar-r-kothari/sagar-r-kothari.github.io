---
layout: post
title: "Android - Kotlin - ListView with ArrayAdapter"
date: 2020-07-01 07:20:00 +0530
categories: Android Kotlin
---

### MainListAdaptor.kt

```kotlin
class ListItem(
    val title: String,
    val subtitle: String
)

class MainListAdaptor(
    private val context: Activity,
    private val list: List<ListItem>
): ArrayAdapter<ListItem>(context, R.layout.list_item_main, list) {

    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
        val inflator = context.layoutInflater
        val view = inflator.inflate(R.layout.list_item_main, null, true)
        // update UI
        view.titleText.text = list[position].title
        view.subtitleText.text = list[position].subtitle
        // ----------
        return view
    }
}
```

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val items = listOf<ListItem>(ListItem("Sagar", "Kothari")
        val adaptor = MainListAdaptor(this, items)
        listView.adapter = adaptor
        listView.setOnItemClickListener() {adaptor, _, position, _ ->
            val item: ListItem? = adaptor.getItemAtPosition(position) as? ListItem
            Toast.makeText(this, "Tapped on ${item?.title ?: "blaa"}", Toast.LENGTH_SHORT).show()
        }
    }
}
```