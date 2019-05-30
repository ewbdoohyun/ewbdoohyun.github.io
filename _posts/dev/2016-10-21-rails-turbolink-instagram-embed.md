---
title: Turbolink - 인스타그램 embed가 load되지 않을 때
layout: post
comments: true
category: [dev, rails]
--- 

이것 때문에 시간을 많이 뺐겼는데, turbolink 세팅을 바꾸면 될 줄 알았다.

instagram.js.coffee

    document.addEventListener "turbolinks:load", (event) ->
        $.getScript("//platform.instagram.com/en_US/embeds.js", () ->
            window.instgrm.Embeds.process()
        )


---

all credit : [http://stackoverflow.com/questions/27408917/instagram-embeds-not-working-when-adding-embeds-dynamically](http://stackoverflow.com/questions/27408917/instagram-embeds-not-working-when-adding-embeds-dynamically)
