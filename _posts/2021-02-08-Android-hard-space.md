---
layout: post
title: "Android - Strings - Hard Space"
date: 2021-02-08 08:00:00 +0530
categories: Android Kotlin 
---

I wanted to keep spaces as prefix and suffix to my app name.

Here is what string value looked like earlier.

```xml
<string name="app_name"> My App Name </string>
```

Unfortunately, strings get trimmed automatically. So, what do we do to keep the space by force?

A simple solution would injecting `&#160;`.

Here is how string value would look like after it.

```xml
<string name="app_name">&#160;My App Name&#160;</string>
```