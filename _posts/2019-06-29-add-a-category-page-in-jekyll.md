---
layout: post
author: "Mikael Asp Somkane"
title:  "Add a category page in Jekyll"
category: [Jekyll]
tags: [category]
---

I use categories in my blog posts to collect the posts that belong together, but
since Jekyll doesn't create category pages and Github pages don't accept plugins
I had to create it myself.

As always I had to read a bunch of forum posts and web pages to get some hints
and [this page][brad] was a good help.

## Category layout

The first thing you do is to create a new layout for categories and you put it
in `` _layouts `` and maybe you can call it `` category.html ``.

Here is my category layout:

###### category.html

{% raw %}
``` liquid
---
layout: default
---

<h2>Category: {{ page.category }}</h2>

{% for post in site.categories[ page.category ] %}

    {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
    {% capture this_month %}{{ post.date | date: "%B" }}{% endcapture %}

    {% unless year == this_year %}
        {% assign year = this_year %}
        <h3>{{ year }}</h3>
    {% endunless %}

    {% unless month == this_month %}
        {% unless forloop.first %} </ul> {% endunless %}
        {% assign month = this_month %}
        <h4>{{ month }}</h4>
        <ul>
    {% endunless %}

    <li><span class="category_date">{{ post.date | date: "%a %d %b %Y" }}</span>
        <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% if forloop.last %} </ul> {% endif %}
{% endfor %}
```
{% endraw %}

It will use page.category from the calling page to find the posts with that
category. It will show headers for year and month and then list the posts of
that month.

<blockquote>
I really wish all languages had something similar to forloop.first and
forloop.last it really helps keeping the code clean. Just a side note.
</blockquote>

Next you need category pages for every category you use. That could possibly be
tedious if you have a bunch. I have 3 so far and I will try to keep them under 10.
But if you speak perl, ruby or python you could go through your post directory and
just create category files with a script.

This is my Jekyll-category page:

###### jekyll.html

``` yaml
---
layout: category
permalink: /categories/Jekyll/
category: Jekyll
---
```
And that's it. It's sets the layout to category, a permalink and a category. The
category parameter is what the category.html layout will use to find the posts.
So you will need a similar file for all your categories.


## Update

[Here][script] I write about the perl script I wrote.

[script]: {% post_url 2019-07-19-perl-script-for-category-and-tag-pages %}
[brad]: https://bradonomics.com/jekyll-categories/
