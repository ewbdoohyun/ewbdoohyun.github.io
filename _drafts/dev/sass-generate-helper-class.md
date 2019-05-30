---
title: Sass
layout: post
category: [dev, rails]
--- 

## Saas List documentation
http://sass-lang.com/documentation/file.SASS_REFERENCE.html#lists


## [Sass Extend][1]


    .message {
      border: 1px solid #ccc;
      padding: 10px;
      color: #333;
    }

    .success {
      @extend .message;
      border-color: green;
    }


---

[Sass Guide][1]


[1]: http://sass-lang.com/guide