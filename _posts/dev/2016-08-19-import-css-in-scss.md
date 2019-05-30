---
layout: post
comments: true
title: Sass 파일에서 css 파일 import
category: [dev, rails]
---

- `@import 'style.css'` 라고 어딘가에서 찾았고, development 환경에서는 문제없이 import된다.
- style.css는 lib/assets/stylesheets 에 위치해 있다.
- production 환경에서는 style.css에서 RoutingError가 발생했다.
- 이유는 잘모르겠지만, `@import 'style'`로 바꾸니, development/production 환경에서 문제없이 import된다.
- `@import`는 sass 문법이다.
    + sass guide를 찾아보니, @import 뒤에 파일명만 쓰면된다. 확장자는 필요없다.
- `@import 'style'`은 app/assets/stylesheets/, lib/assets/stylesheets/, vendor/assets/stylesheets/ 폴더에서 style.scss, style.sass, style.css 파일을 찾는다.

[Sass guide](http://sass-lang.com/guide)
