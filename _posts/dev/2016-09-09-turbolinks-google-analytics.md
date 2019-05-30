---
title: Turbolinks & Google analytics
layout: post
comments: true
category: [dev, rails, google analytics]
---

Turbolink는 AJAX로 `<body>` tag내 컨텐츠만 바꿔서 page loading time을 줄여준다.
GA는 page load시 마다 서버로 pageview 데이터를 전달하기 때문에 Turbolink를 사용하면 ajax로 이동한 page에 대해서는 GA로 분석 할 수 없다.

`turbolinks:load` 이벤트가 발생될 때 마다 GA로 pageview를 보내는 javascript code를 추가해서 해결할 수 있다.

application.html.erb

    <head>
      <%= render 'layouts/google_analytics' %>
    </head>

laytouts/_google_analytics.html.erb
{% highlight javascript %}
<% if Rails.env.production? %>
  <script>
    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
    (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
    m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

    ga('create', 'XX-XXXXXXXX-X', 'auto');
    ga('send', 'pageview');
  </script>

<% else %>
  <script>
    function ga () {
      var params = Array.prototype.slice.call(arguments, ga.length);
      console.log("GoogleAnalytics: " + params);
    };
  </script>
<% end %>
{% endhighlight %}

app/assets/javascripts/google_analytics.js.coffee
{% highlight javascript %}
document.addEventListener "turbolinks:load", (event) ->
  if typeof ga is "function"
  ga("set", "location", event.data.url)
  ga("send", "pageview")
{% endhighlight %}


---
[credit: sheharyar](https://sheharyar.me/blog/using-google-analytics-in-rails-4-with-turbolinks/)

[credit: scottwb](http://stackoverflow.com/questions/18945464/rails-4-turbolinks-with-google-analytics/25050377#25050377)
