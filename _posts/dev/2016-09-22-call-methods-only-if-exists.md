---
title: tip - nil이 아닌경우에 method를 실행
layout: post
comments: true
category: [dev, ruby, rails]
--- 

object가 nil이 아닌 경우에만 method를 실행하고 싶을 때가 있다.

    self.send(:method) if self.respond_to?(:method)

Rails에는 `.try()`가 있다.

---

[http://stackoverflow.com/questions/15787610/call-method-only-if-it-exists](http://stackoverflow.com/questions/15787610/call-method-only-if-it-exists)
[http://everydayrails.com/2011/04/28/rails-try-method.html](http://everydayrails.com/2011/04/28/rails-try-method.html)
