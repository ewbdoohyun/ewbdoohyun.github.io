---
title: Rails - 20자 이상 자르고 말줄임표 붙이기
layout: post
comments: true
comments: true
category: [dev, rails]
--- 

Rails의 [truncate][1]를 쓰면, 20자 이상인 경우 20자까지만 보여주고 말줄임표로 바꿔준다.
말줄임표를 어떤 문자로 할지도 설정할 수 있다.

    truncate("Once upon a time in a world far far away", length: 17)
    # => "Once upon a ti..."

    truncate("Once upon a time in a world far far away", length: 17, separator: ' ')
    # => "Once upon a..."

    truncate("And they found that many people were sleeping better.", length: 25, omission: '... (continued)')
    # => "And they f... (continued)"


---


[1]: http://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html#method-i-truncate
