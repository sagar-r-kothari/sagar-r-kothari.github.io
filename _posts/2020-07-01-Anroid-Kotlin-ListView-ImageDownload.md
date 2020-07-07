---
layout: post
title: "Android - Kotlin - ListView - download image"
date: 2020-07-02 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```
dependencies {
    ...
    implementation 'com.github.bumptech.glide:glide:4.9.0'
    ...
}
```

### MainListAdaptor.kt

```kotlin
class ListItem(
    val title: String,
    val subtitle: String,
    val imageURL: String
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

        // Download Image and set it in listView
        Glide.with(view)  // glide with this view
            .load(list[position].imageURL) // download using this url
            .override(250, 250) // set this size
             .placeholder(R.mipmap.ic_launcher) // default placeHolder image
             .error(R.mipmap.ic_launcher) // use this image if faile
            .into(view.imageView) // set 

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
        val items = listOf<ListItem>(ListItem("Sagar", "Kothari", "https://google.com/some/image.png")
        val adaptor = MainListAdaptor(this, items)
        listView.adapter = adaptor
        listView.setOnItemClickListener() {adaptor, _, position, _ ->
            val item: ListItem? = adaptor.getItemAtPosition(position) as? ListItem
            Toast.makeText(this, "Tapped on ${item?.title ?: "blaa"}", Toast.LENGTH_SHORT).show()
        }
    }
}
```