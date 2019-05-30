---
title: Rails HABTM convention과 다른 table을 이용하기
layout: post
comments: true
category: [dev, rails]
--- 

# 0


    has_and_belongs_to_many :watching, :class_name=>"Person", :join_table => :watch_people_projects


[solution credit](http://tedhealey.blogspot.kr/2011/02/habtm-rails-with-different-table-name.html)





