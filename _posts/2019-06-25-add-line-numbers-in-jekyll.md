---
layout: post
author: "Mikael Asp Somkane"
title:  "Add line numbers in Jekyll code examples"
category: [Jekyll]
tags: [css]
---

Sometimes I want to refer to a certain line in a code example and decided to add
line numbers. To get them just add the following lines to `` _config.yml ``.

```yaml
kramdown:
  syntax_highlighter_opts:
    block:
      line_numbers: true
```

If you test it you will se that if you have more than one example in a page the
left column will have different width depending on the width of the code on the
right. I fixed that with a little bit of css that I explain in [this blog
post]({{ site.baseurl }}{% post_url 2019-06-22-add-css-to-jekyll %}).

