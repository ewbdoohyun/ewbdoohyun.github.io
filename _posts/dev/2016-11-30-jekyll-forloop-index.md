---
title: Jekyll에서 forloop index 알아내기
layout: post
comments: true
category: [dev, jekyll]
--- 

jekyll은 liquid 문법을 사용합니다.

for loop에서 몇번 째 iteration인지 아는 방법

{%raw%}
{% for category in page.categories %}
    {{ forloop.index }}
{% endfor %}
{% endraw %}

`forloop.index` 는 1 부터 시작합니다. (왜 0이 아닌거지?)


---
[liquid reference - forloop][1]

[1]: https://help.shopify.com/themes/liquid/objects/for-loops
