---
layout: post
title:  "Jekyll - Generate Categories Page"
date:   2019-04-12 14:00:00 +0530
categories: Jekyll
---

Save following as `Categories.html` at root of blog folder.

<pre>
---
layout: page
permalink: /categories/
title: Categories
---
&#60;div id="archives"&#62;
&#123;&#37; for category in site.categories &#37;&#125;
  &#60;div class="archive-group"&#62;
    &#123;&#37; capture category_name &#37;&#125;&#123;&#123; category | first &#125;&#125;&#123;&#37; endcapture &#37;&#125;
    &#60;div id="#&#123;&#123; category_name | slugize &#125;&#125;"&#62;&#60;/div&#62;
    &#60;p&#62;&#60;/p&#62;
    
    &#60;h3 class="category-head"&#62;&#123;&#123; category_name &#125;&#125;&#60;/h3&#62;
    &#60;a name="&#123;&#123; category_name | slugize &#125;&#125;"&#62;&#60;/a&#62;
    &#123;&#37; for post in site.categories[category_name] &#37;&#125;
    &#60;article class="archive-item"&#62;
      &#60;h4&#62;&#60;a href="&#123;&#123; site.baseurl &#125;&#125;&#123;&#123; post.url &#125;&#125;"&#62;&#123;&#123;post.title&#125;&#125;&#60;/a&#62;&#60;/h4&#62;
    &#60;/article&#62;
    &#123;&#37; endfor &#37;&#125;
  &#60;/div&#62;
&#123;&#37; endfor &#37;&#125;
&#60;/div&#62;
</pre>