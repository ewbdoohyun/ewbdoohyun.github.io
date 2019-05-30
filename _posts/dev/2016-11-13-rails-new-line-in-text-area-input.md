---
title: Rails - text-area 줄바꿈 입력값 보여주기
layout: post
comments: true
category: [dev, rails]
--- 

[simple_format][1] 을 이용하면, text-area에서 받은 줄바꿈('\r')을 쉽게 보여줄 수 있다.

    //form
    form_for comment
        f.text_area :body
        ...

    //show
    <%= simple_format comment.body %>



[1]: http://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html#method-i-simple_format
