---
title: Jekyll에서 다른 post 링크를 넣는 방법
layout: post
comments: true
category: [dev, jekyll]
--- 


Post에서 다른 post(2016-11-30-filename.md)의 링크를 넣는 방법.

{% raw %}
    {% post_url 2016-11-30-filename %}
{% endraw %}

Jekyll documentation에서는 sub directory가 있는 경우 subDirectory를 명시하도록 되어있지만, 시도해보니, directory path를 넣으면 compile error가 발생하고 subdirectory없이 파일명으로만 하였더니, 링크가 제대로 생성되었다.



post_url을 이용해서 링크를 만드는 방법

{% raw %}
    [link_text]({% post_url 2016-11-30-filename %})
{% endraw %}


---
[jekyll doc][1]
[stackoverflow][2]

[1]: http://jekyllrb.com/docs/templates/#post_url
[2]: http://stackoverflow.com/questions/4629675/jekyll-markdown-internal-links
