---
title: Rails - 유저 role 별로 cache 사용
layout: post
comments: true
category: [dev, rails]
--- 


## 유저 role : 일반 유저와 admin 유저


Rails의 cache는 4.0부터 key based cache를 지원합니다

key값은 `cache` method에 넘겨주는 object의 updated_at 값을 기반으로해서, 해당 object가 수정되면 자동으로 cache가 update됩니다.


    <% cache project do %>
      <b>All the topics on this project</b>
      <%= render project.topics %>
    <% end %>

cache key

    views/projects/123-20120806214154/7a1156131a6928cb0026877f8b749ac9
      ^class   ^id ^updated_at    ^template tree digest



유저의 role 등 view가 바뀌는 dependency를 가진 모든 것을 array형태로 `cache` method에 넘겨줄 수 있습니다.

    <% cache [admin_user?, project] do %>
      <b>All the topics on this project</b>
      <%= render project.topics %>
      <%= render 'admin' if admin_user? %>
    <% end %>


cache key

    views/false/projects/123-20120806214154/7a1156131a6928cb0026877f8b749ac9
            ^ admin_user? value


cache key를 보면 `admin_user?` 라는 값에 따라 다른 cache key가 발급되고, 다른 cached page가 로딩된다는 것을 알 수 있습니다.




---

(Rails API - cache)[1]
(Rails의 key-based cache에 대한 DHH의 blog 글)[2]

[1]: http://api.rubyonrails.org/classes/ActionView/Helpers/CacheHelper.html#method-i-cache
[2]: https://signalvnoise.com/posts/3113-how-key-based-cache-expiration-works
