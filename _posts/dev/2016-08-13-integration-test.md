---
layout: post
comments: true
title: Rails Integration Test
category: [dev, rails]
---

- ```rails g integration_test <NAME>```
- `get <path>, <params_hash>
  - get search_path, search: "keyword1"
- Testing XHR (AJAX) requests
  {% highlight ruby %}
    test "ajax request" do
      article = articles(:one)
      get article_url(article), xhr: true
      assert_equal 'hello world', @response.body
      assert_equal "text/javascript", @response.content_type
  end
  {% endhighlight %}
- Model 생성 / 삭제 / 변경없음 확인
  -
  {% highlight ruby %}
  assert_difference 'Model.count' do
    post ---
  end
  {% endhighlight %}
- `.reload` DB의 값을 다시 읽어옴



----
참고링크

- [How We test Rails Application](https://robots.thoughtbot.com/how-we-test-rails-applications)
- [Rails guide ](http://edgeguides.rubyonrails.org/testing.html#testing-xhr-ajax-requests)
