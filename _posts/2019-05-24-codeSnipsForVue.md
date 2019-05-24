---
layout: post
title:  "Code Snips for Vue.js"
date:   2019-05-24 11:00:00 +0530
categories: Veu.js
---

#### For development purpose

```html
<body>
	....
	....
	....
	<script src="https://cdn.jsdelivr.net/npm/vue"></script>
</body>
```

#### For deployment/distribution purpose

```html
<body>
	...
	...
	...
	<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</body>
```

```js
var app = new Vue({ // Create Veu
	el: '#app', // using element having 'app' as 'id'
	data: { // data for that element
		product: 'Socks',
		description: 'A pair of warm, fuzzy socks',
		image: './img/vmSocks-green.png',
		inventory: 10,
		onSale: true,
        details: ["80% cotton", "20% polyester", "Gender-neutral"]
	}
})
```