---
layout: post
comments: true
title: Jekyll - category array를 sub-category처럼 사용하기
category: [dev, jekyll]
---

- `dev` category의 sub category를 만들려고 했다.
- `dev` category 페이지에서 sub category별로 post title을 모아서 보여주려고 했다.
- Jekyll의 category관련 variable
  - `site.categories`
  - `site.categories[CATEGORY_NAME] or site.categories.CATEGORY_NAME`
  - `page.categories`
- 각 post의 front matter에서 category를 지정할 수 있다.
  - {% highlight text %}
      ---
      layout: post
comments: true
      title: Rails Integration Test
      category: [dev, jekyll]
      ---
    {% endhighlight%}
- Jekyll은 template language로 [Liquid](https://shopify.github.io/liquid/)를 쓴다.
- 폴더를 보기 좋게 만든다. (front matter에 category가 정의되어 있지 않으면, _posts 폴더 내의 폴더 구조를 category로 본다.
  - _posts
    - biz
    - book
    - dev
- dev category 페이지는 `categories/dev`로 접근하게 한다.
  - `/categories/dev/index.html` 을 만든다.
- /categories/dev/index.html
  - front matter에 category를 dev로 정의
    {% highlight text %}
      ---
      layout: default
      Title : Dev. Category
      category: dev
      ---
    {% endhighlight %}
  - 빈 array 만들기
    {% highlight liquid %}
        {%raw%}
          {% assign sub_categories = "" | split: "" %}
        {%endraw%}
    {% endhighlight %}
  - front matter에 정의된 category 내의 모든 post에 대해 iteration
    {% highlight liquid %}
        {%raw%}
          {% for post in site.categories[page.category] %}
        {%endraw%}
      {% endhighlight %}
  - dev category가 아닌 경우 sub_categories array에 추가
    {% highlight liquid %}
        {%raw%}
          {% if category != "dev" %}
            {% assign sub_categories = sub_categories | push: category %}
          {% endif %}
        {%endraw%}
      {% endhighlight %}
  - 마지막으로 중복된 sub_category를 없애준다.
    {% highlight liquid %}
      {%raw%}
        {% assign sub_categories = sub_categories | uniq %}
      {%endraw%}
    {% endhighlight %}
- 다른 category에서도 sub_category 배열을 만드는 코드를 쓸 거라서 `/inclues/get_sub_category.html`로 따로 만든다.
  - `/categories/dev/index.html`
    - {% highlight liquid %}
      {%raw%}
        {% include get_sub_category.html %}
      {%endraw%}
    {% endhighlight %}
  - `/includes/get_sub_category.html`
    - {% highlight liquid %}
      {%raw%}
        {% assign sub_categories = "" | split: "" %}
        {% for post in site.categories[page.category] %}
          {% for category in post.categories %}
            {% if category != "dev" %}
              {% assign sub_categories = sub_categories | push: category %}
            {% endif %}
          {% endfor %}
        {% endfor %}
        {% assign sub_categories = sub_categories | uniq %}
      {%endraw%}
    {% endhighlight %}

---
- Jekyll에 대해서 알게된 것 정리
- liquid tag를 escaping 하는 법
  - raw tag 사용 {% raw %}{% raw %}{% endraw %}  {{ "{% endraw " }}%}
    - 이 방법으론 endraw 를 escaping 하지 못함
    - endraw는 {%raw%}{{ "{% endraw " }}%}{%endraw%} 이 방법으로 escaping.
- Array empty check
  - {% highlight liquid %}
      {%raw%}
        {% assign size = sub_categories | size %}
        {% if size == 0 %}
      {%endraw%}
    {% endhighlight %}

----
참고링크

- [Thomas Bradley - jekyll cast](https://www.youtube.com/user/acinteractivedesign/search?query=jekyll)
- [jekyll variable](https://jekyllrb.com/docs/variables/)
- [jekyll & liquid cheat sheet](https://gist.github.com/smutnyleszek/9803727)
- [Escaping liquid tags](http://taylor.fausak.me/2013/02/03/escaping-liquid-tags/)
- [Liquid](https://shopify.github.io/liquid/)
- [Checking for empty](https://github.com/jekyll/jekyll/issues/2538)
