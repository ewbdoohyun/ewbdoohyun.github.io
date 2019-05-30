---
title: Preload/Eagerload Nest Association
layout: post
comments: true
category: [dev, rails]
--- 


## Preload Nest Association

    Menu.includes(submenus: :items)



## conditional preload

scope은 preload에 사용할 수 없으니, lambda를 이용해서 condition을 만족하는 association을 생성.

    class SubMenu
        has_one :best_item, -> {where("rank = ?", 1)}, class_name: "Item"


    Menu.includes(submenus: :best_item)

---

[http://stackoverflow.com/questions/24397640/rails-nested-includes-on-active-records][1]

[1]: http://stackoverflow.com/questions/24397640/rails-nested-includes-on-active-records
