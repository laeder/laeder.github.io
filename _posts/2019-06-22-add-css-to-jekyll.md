---
layout: post
author: "Mikael Asp Somkane"
title:  "Add CSS to Jekyll"
category: [Jekyll]
tags: [css]
---

Adding a small piece of CSS to my blog was way harder then I had expected. In
fact, it's not very difficult if you know how to do it. The problem was that
every forum or blog post had different answers to how to do it and everything
was wrong. It wasn't until I found [this page][sass converter] that I finally
figured out how to do it.

## Create partials

The first you create is a partial. A partial is a scss-file that you can compile
togheter with other partials into one css-file using Jekyll's sass-converter and
it has no YAML front matter. I just need a few lines of css to adjust the table
used for code highlightning. 
### The _sass directory

You place your partials in the directory `` _sass ``. Like me, maybe you just
need one partial. Partial file names starts with an underscore, my file is called
`` _rouge.scss `` and it looks like the example below. The reason I named it that
way is because it has scss for Rouge, the highlighter that Jekyll use, at this
moment. If I had som changes for navigation I would add one and name it ``
_navigation.scss ``.


#### _rouge.scss
``` scss
.highlight {
    table {
        td.rouge-gutter { width: 7%; }
        td.rouge-code { width: 93%; }
   }
}

.highlight {
    table { margin: 0px; }
}
```

Line 3 sets the width of the line number column to 7% and the code column to
93%. The theme set the width of the whole table to 100% so this was a way to
fix the widths. Line 9 sets the margin of the table to 0% since the theme has a
bottom margin at 20px, but that looked bad in the code examples.


## The main file

The main file is a file with a YAML front matter, an empty line and an import
line for every partial you want to include in the css-file. The reason for the
front matter is for Jekyll to read it and compile it. Otherwise it would just
skip it.

### The assets/css directory

You put your main file in the `` assets/css `` directory and as soon as you save
it Jekyll will compile it and create a css-file with the same name in ``
_site/assets/css ``. My theme alredy has a style.css so I named my main file ``
main.scss `` and the result was `` main.css `` next to style.css.

#### main.scss
``` scss
---
---

@import "rouge";
```

As you can see, you don't write the underscore in the import line, nor the
extension. 

## Include the css-file

The last step is to include the new css-file in the html-code and that may
differ from theme to theme or if you have built your own layout.

I just added a line into `` _layout/default.html `` which might be the case for
all GitHub pages themes. What I did was to copy the line with the theme's
css file and just change it to my new css file like below.

{% raw %}
``` html
<link rel="stylesheet" href="{{ '/assets/css/style.css?v=' | append: site.github.build_revision | relative_url }}">
<link rel="stylesheet" href="{{ '/assets/css/main.css?v=' | append: site.github.build_revision | relative_url }}">
```
{% endraw %}

## Summary

Both Github pages and Jekyll has been around for years and has changed over time
and that is probably why there are so many different answers out there.

[sass converter]: https://github.com/jekyll/jekyll-sass-converter/tree/master/docs

