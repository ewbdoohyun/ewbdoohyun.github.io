---
layout: post
comments: true
title: Rails(레일즈) scope
category: [dev, rails]
---

- `scope`은 자주 사용하는 query를 method형태로 사용 할 수 있게 해준다.
- `scope`은 `def self.` class method와 같다.
- argument를 넘겨 줄 수도 있다.
    + `scope :created_before, ->(time) { where("created_at < ?", time) }`
- `AND` 컨디션으로 여러 개의 scope을 합칠 수도 있다.
    {% highlight ruby%}
    class User < ApplicationRecord
      scope :active, -> { where state: 'active' }
      scope :inactive, -> { where state: 'inactive' }
    end
 
    User.active.inactive
    # SELECT "users".* FROM "users" WHERE "users"."state" = 'active' AND "users"."state" = 'inactive'
    {% endhighlight %}
- `OR` 컨디션은 Rails4 기준으로 없다.
    + Rails5에 `.or`이 추가된다.
    + Rails4에서는 `.arel_table`을 사용한다.
        {% highlight ruby %}
            t = Task.arel_table
            Task.where(t[:complete].eq(true).or(t[:complete].eq(false))
        {% endhighlight %}
