---
title: Omniauth Facebook
layout: post
comments: true
category: [dev, rails]
--- 

## Codes

[coderwall.com][3] 참고


## Errors & Soutions

1. NoAuthorizationCodeError

    OmniAuth::Strategies::Facebook::NoAuthorizationCodeError must pass either a code parameter or a signed request (via signed_request parameter or a fbsr_XXX cookie)


`FB.init` 에 `cookie: true` 옵션을 넣어주니 해결되었다.


## with turbolink

facebook js sdk에서는 sdk를 초기화할 때, body안에 fb-root라는 dom element를 만듬
turbolink로 이동하면 이 fb-root가 사라진다.

가장 간단한 방법은 모든 turbolink request마다 sdk를 다시 load하면 되는 거지만, 그렇다면 turbolink를 쓰는 이유가 무색해진다.

turbolinks와 다른 javscript library와의 compatibility를 정리해둔 (http://reed.github.io/turbolinks-compatibility/)[1] 의 code를 보면 아래와 같이 작동한다.

- tubolink request가 시작되기 전에 `fb-root` element를 window.fbRoot에 저장
- turbolink로 새로운 dom이 load되면 저장된 window.fbRoot를 이용해서 `fb-root`를 복원


### Turbolink 5

turbolink3에서 turbolink 5로 넘어오면서, event 이름들이 바뀌었다.
page:chage , page:load --> turbolinks.load
page:fetch --> turbolinks:request-start

[turbolink github][2]에 가면 jquery event를 turbolink5의 event로 바꿔주는 script가 있다.



---


[1]: http://reed.github.io/turbolinks-compatibility/facebook.html
[2]: https://github.com/turbolinks/turbolinks/blob/master/src/turbolinks/compatibility.coffee#L25
[3]: https://coderwall.com/p/bsfitw/ruby-on-rails-4-authentication-with-facebook-and-omniauth
