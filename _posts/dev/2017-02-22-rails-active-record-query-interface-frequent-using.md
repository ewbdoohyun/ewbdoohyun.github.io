---
title: [Rails/ActiveRecord] 자주 쓰는 API
layout: post
comments: true
category: [dev, rails]
--- 


## .find_each

모든 record에 대해서 일괄작업시 자주 사용

    Item.find_each do |i|
        i.update_weekly_ranks
    end


