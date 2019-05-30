---
title: Rails(레일즈) bootstrap 세팅.
layout: post
category: [dev, rails, bootstrap]
---
[bootstrap-sass](https://github.com/twbs/bootstrap-sass) gem 초기 세팅 과정이다.

**Stylesheets**

- application.css를 application.scss로 바꾼다.
- application.scss에 추가.
  {% highlight ruby %}
    @import "bootstrap-sprockets";
    @import "bootstrap";  
  {% endhighlight %}
  
- application.css에 있었던 require문 제거
  {% highlight ruby %}
    *= require_self
    *= require_tree .
  {% endhighlight %} 

**Javascripts**

- application.js에 추가.
  {% highlight ruby %}
    //= require jquery
    //= require bootstrap-sprockets
  {% endhighlight %}