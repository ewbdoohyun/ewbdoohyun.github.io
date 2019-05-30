---
title: params를 assign할 때 생기는 현상
layout: post
category: [dev, rails]
---

- 어떤 이유에서건 `params` 를 controller에서 변경할 필요가 생겼다.
- 다음 code는 nil object에 해당 operator가 없다는 error가 발생한다.
    {% highlight ruby %}
        params = params[:key]
    {% endhighlight %}
- ruby에서는 assign이 되기 전에 우선 nil로 초기화 한다.
    + `a = a` 를 수행하면 `a`는 `nil`이 된다.
- params는 `ActiveController::StrongParameter`에 정의된 method다.
- `parmas =`는 params라는 local-variable을 nil로 초기화 시킨다.
- `params[:key]`를 수행할 때 params는 더이상 method가 아닌 local-variable이다.
- controller에서 아래 코드를 테스트해 보면 확실하게 알 수 있다.
    {% highlight ruby %}
    <in any controller>
        def index
            debugger
        end

    <byebug console>
    (byebug) defined? params
    "method"
    (byebug) params
    {"controller" => "tests", "action"=>"index"}
    (byebug) params = params
    nil
    (byebug) params
    nil
    (byebug) defined? params
    "local-variable"
    {% endhighlight %}


[stackoverflow](http://stackoverflow.com/questions/14525734/pass-url-params-hash-in-controller-to-another-method-becomes-nil)