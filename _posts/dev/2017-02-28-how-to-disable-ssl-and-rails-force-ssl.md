---
title: Rails - SSL 제거시 발생한 문제 (config.force_ssl의 비밀)
layout: post
comments: true
category: [dev, rails]
--- 

[stackoverflow - Disabling SSL for a Heroku App][1] 에 모든 credit이 있습니다.

## 문제와 현상

- heroku에서 ssl을 설정했다가, ssl 인증서를 지워버림
- 그 뒤부터 bebetem.herokuapp.com 으로는 접속이 되는데, bebetem.com 으로는 http를 명시해도 https로 redirect되면서 접속이 안됨
- `heroku logs -t` 에서 해당 redirection이 로그로 남지 않음

## 시도한 방법

- 우선 rails의 `config.force_ssl=false`로 변경함
- DNS에서 https://www.bebetem.com 으로 forwarding을 해놨기 때문에, 그 때문인 것 같아서 http로 forwarding을 바꿨으나 해결되지 않음
- heroku에서 ssl을 사용하면 dns target이 bebetem.herokuapp.com 에서 bebetem.com.herokdns.com 으로 변경되었었음. 그래서 다시 원래와 같이 bebetem.herokuapp.com으로 dns cname을 변경하였으나 dns를 찾지 못해서 에러가 발생

## 해결책

- SSL설정시 rails에서 `config.force_ssl=true` 를 설정하였음
- 이 설정은 `Strict Transport Security` header에 max-age 값을 1년으로 설정해버림
- 이 header값은 브라우저가 1년동안은 서버로 접속할 때 https를 사용하도록 강제함
- 그래서 아무리 heroku logs -t 로 로그를 보아도 http에서 https로 redirect되는 로그를 찾아볼 수가 없었음


## trick

application_controller.rb

    before_filter :expire_hsts
    private
      def expire_hsts
        response.headers["Strict-Transport-Security"] = 'max-age=0'
      end


브라우저의 cache를 삭제하고, bebetem.com에 접속하니 http로 접속이 가능!





---


[1]: http://stackoverflow.com/questions/17776530/disabling-ssl-for-a-heroku-app
