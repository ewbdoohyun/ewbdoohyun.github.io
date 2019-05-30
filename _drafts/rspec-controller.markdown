---
title: rspec-controller
layout: post
---


- configuration
    - Controller test에서 view 사용
        + 
        {% highlight ruby %}
            describe MenuItemsController, "blahblah" do
                integrate_views
                fixtures :menu_itmes
        {% endhighlight %}
- Use moca gem for stub/mock 

- test redirect
    `response.should redirect_to(menu_items.path)`
- test flash message
    `flash[:notice].should_not be_nil`
- test model save safely
    `assigns[:menu_itme].should be_new_record`
- code를 break시켰는데, test가 break되지 않았다면, 적절한 test가 없다는 것.
- controller의 action들이 꽤 비용이 있다.
    + performance를 위해 한 개의 test에 여러개의 assertion을 사용



- [rails rspec guide doc](http://betterspecs.org/ko)
