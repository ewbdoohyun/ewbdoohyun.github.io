---
title: Rails - View에서 현재 controller 이름 사용하기
layout: post
comments: true
category: [dev, rails]
--- 


controller에 따라서 뭔가 분기가 필요할 때.

Rails는 `controller_name`이라는 variable에 현재 controller 이름을 저장하고 있다.

    <%= if controller_name == 'blogs' %>
        ~~
    <% end %>




