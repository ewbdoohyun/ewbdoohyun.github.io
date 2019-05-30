---
layout: post
comments: true
title: model이 없는 form의 입력을 validation 이후에도 유지하기.
category: [dev, rails]
---

Rails는 많은 일을 해줘서, 가끔은 이게 Rails에서 해주는것인지 브라우저에서 해주는 것인지 헷갈릴 때가 있다.

비밀번호가 틀렸을 때, id값을 유지하는 것도 model을 사용한다면 form에 인스턴스를 사용하는 것 만으로 해결된다.


< in Session Controller file >

{% highlight ruby %}
def create
  if [success]
  else
    render 'new'
  end
end
{% endhighlight %}

< in new.html.erb >

{% highlight ruby %}
	form @user do |f|
	...

{% endhighlight %}


Ara house의 로그인은 별도의 model없이 session을 이용하는데,
그래서 params에 email이 있는지 확인하고, 있으면 다시 input field에 넣어주는 작업이 필요하다.
이건 Rails가 해주지 않는 것 같다.

{% highlight ruby %}
	<%= f.email_field :email, value: if parmams.key?("email") then params[:email] else "" end %>
{% endhighlight %}


login 실패 시, email이 input에 그대로 남아있는지 확인하는 test를 login integration test에 추가한다.

{% highlight ruby %}
users_login_test.rb

	...
	assert_select 'form input[type=email][value="123"]'
	...

{% endhighlight %}

-----
<small>참고링크</small>

<small>[Stackoverflow](http://stackoverflow.com/questions/4129229/rails-restoring-contents-of-non-model-form-that-uses-form-tag)</small>
<small>[Stackoverflow - test input value](http://stackoverflow.com/questions/17710948/how-to-assert-the-value-of-an-input-element-with-assert-select)</small>
