---
layout: post
comments: true
title: Bootstrap collapse의 event handler 사용하기
category: [dev, bootstrap]
---

우선 만든것을 먼저 보여드린다.

감춰진 상태
![collapsed]({{ site.baseurl }}/assets/collapse1.png)

펼쳐진 상태
![expanded]({{ site.baseurl }}/assets/collapse2.png)

bootstrap collapse에서 저 화살표를 만들어주지 않는다.
대신에 감춰지거나, 펼쳐질 때 event handler를 제공한다.

bootstrap 2와 bootstrap 3의 event 이름이 달라졌다.
저 처럼 bootstrap3 이용하면서 bootstrap 2 manual 보는 사람이 없기를...

{% highlight js %}
  $('.collapse').on('show.bs.collapse', function () {
      $(this).siblings("a").find("h4 > i").removeClass("fa-angle-right").addClass("fa-angle-down");
  });
  $('.collapse').on('hidden.bs.collapse', function () {
      $(this).siblings("a").find("h4 > i").removeClass("fa-angle-down").addClass("fa-angle-right");
  });
{% endhighlight%}

`siglings("a")`로 collapse되지 않는 부분을 찾고, `find("h4 > i")`로 fond awsome의 i tag를 선택한다.
i tag의 class를 바꿔주면 화살표가 바뀌게 된다.

보여줄 때는 화살표가 먼저 바뀌고 보여지는게 왠지 익숙하고, 감춰질 때는 감춰지고 난 뒤에 화살표가 바뀌는게 익숙하다.
(왜 그런지는.. 잘 모르겠지만)

`show`, `hide` 는 animation을 기다리지 않고 바로 전달되는 event이고, `shown`, `hidden`은 animation이 다 완료된 이후에 전달되는 event이다.

적절하게 `show`와 `hidden` event를 사용했다.

-----
<small>Google it!</small>

<small>bootstrap 3 collapse</small><br>
<small>jquery addClass</small>


