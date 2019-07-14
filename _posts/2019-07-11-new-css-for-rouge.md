---
layout: post
author: "Mikael Asp Somkane"
title:  "New css for Rouge"
category: [Jekyll]
tags: [css, rouge, hightlight]
---

I wasn't totally satisfied with the look of code examples so I rewrote and added
some css to `` _rouge.scss ``.

#### _rouge.scss

``` scss
.highlight {
    table {
        line-height: 1.3;
        margin: 0px;

        td {
            padding: 0px;
        }
        
        td.rouge-gutter {
            width: 5%;
            text-align: right;
            pre.lineno {
                color: #006666;
                padding: 6px;
                border-left: 0px;
                border-top: 0px;
                border-right: 1px solid #666666;
                border-bottom: 0px;
            }
        }

        td.rouge-code {
            width: 95%;
            border: 0px;
            pre {
                border: 0px;
                padding: 6px 6px 6px 12px;
            }
        }
    }
}
```

On line 10 is the rouge-gutter, which is the TD where the line numbers are. I
removed all the borders except the one on the right. I also right aligned the
text and changed the color. I also made som changes to width and padding.

On line 23 is the rouge-code, and that's the TD for the code. I removed all the
borders and added som extra space on the left side to make it easier to read.



