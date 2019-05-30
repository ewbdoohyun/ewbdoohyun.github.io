---
title: Tips - Rails(레일즈) active model serializer
layout: post
comments: true
category: [dev, rails]
---

- Active record를 사용한다면, `.as_json only:` 로 원하는 attribute만 json으로 만들 수 있다.
  {% highlight ruby %}
  Item.first.as_json only: [:id, :name]
  => {"id" => 1, "name" => "abcde"}
  {% endhighlight %}

- Attribute를 조합해서 새로운 data를 만들어야 된다면, `active_model_serializer`를 사용하자.

- 초기 세팅은 [active model serializer github](https://github.com/rails-api/active_model_serializers) 참고

    {% highlight ruby %}
    class TaskSerializer < ActiveModel::Serializer
        def Date 
            object.completed_at.strftime("%Y-%m-%d")
        end
        attributes :Date
    end
    {% endhighlight %}

- serializer class에 원하는 data를 return하는 method를 만들고, `attributes`로 선언해주면, 해당 data를 가진 json을 serializer로 만들 수 있다.

  {% highlight ruby %}
    render json: @task
  {% endhighlight %}

- `render` 이외에서 json data를 만들 수도 있다.
    {% highlight ruby %}
        @data = ActiveModel::ArraySerializer.new(Task.completed, each_serializer: TaskSerializer).to_json
        or
        @datum = TaskSerializer.new(Task.first).to_json
    {% endhighlight %}

- `root: false` option을 사용하면 model_name으로 된 key를 없앨 수 있다.
    {% highlight ruby %}
      render @item
      ==> {item: [~~~]}

      render @item, root: false
      ==> {[~~~]}
    {% endhighlight %}
- 천자리구분점을 위해 `number_with_delimeter`를 사용할 필요가 있다면, helper module을 include해준다.
    {% highlight ruby %}
    include ActionView::Helpers::NumberHelper
    {% endhighlight %}

- `active model serializer`는 0.9버전과 0.10버전이 호환되지 않는다. 사용하는 버전에 맞는 api 문서를 참고하자.


---
Reference
- [active model serializer 0.9 api](https://github.com/rails-api/active_model_serializers/tree/0-9-stable)
- [stack overflow - How to use the “number_to_currency” helper method in the model rather than view?](http://stackoverflow.com/questions/5176718/how-to-use-the-number-to-currency-helper-method-in-the-model-rather-than-view)
- 
