---
title: Rails bug - current_page? No route matches
layout: post
comments: true
category: [dev, rails]
--- 


`current_page?` 사용시 No route 에러 발생


error
    current_page?(controller: "users", action: "show")

bug fix
    current_page?(controller: "users", action: "show", id: current_user.id)

아마도 url에서 id값을 가져오는데, root 인 경우엔 id 값이 없어서 에러가 발생
추가적으로 id 값을 전달해서 해결



---

[stackoverflow][1]

[1]: http://stackoverflow.com/questions/5627958/no-routes-matches-when-using-current-page-in-rails-3
