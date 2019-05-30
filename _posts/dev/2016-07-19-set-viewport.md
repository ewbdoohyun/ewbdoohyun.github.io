---
layout: post
comments: true
title: 모바일 Layout - View port
category: [dev, html]
---


Viewport는 웹페이지에서 눈에 보이는 영역을 말한다.

Desktop기준으로 만들어 둔 web page는 모바일의 viewport보다 훨씬 큰데,
모바일 브라우저는 이걸 화면크기에 맞게 축소시켜서 보여준다.

{% highlight html %}
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1">

{% endhighlight %}

HTML5부터 viewport를 개발자가 제어 할 수 있게 되었다.

`width=device-width`는 모바일 기기의 화면 크기를 web page의 width로 맞추고,

`initial-scale=1.0`은 모바일 브라우저에서 처음 로딩 될 때, 축소하거나 확대하지 않게한다.

Viewport 적용 전
![before-viewport](http://www.w3schools.com/css/img_viewport1.png)

Viewport 적용 후
![after-viewport](http://www.w3schools.com/css/img_viewport2.png)


-----
<small>참고링크</small>

<small>[w3schools viewport](http://www.w3schools.com/css/css_rwd_viewport.asp)</small>
<small>[Responsive design with twitter bootstrap](http://www.telerik.com/blogs/responsive-design-with-twitter-bootstrap)</small>
