---
layout: post
comments: true
title: Ruby - Naver 검색광고 API
category: [dev, rails]
---

- API Access License 와 Secret Key를 발급받자. ([네이버 검색광고](https://manage.searchad.naver.com/) 에 들어가 도구 > API 사용관리)
- Header에 다음을 추가해야 한다.
  - X-Timestamp
  - X-API-KEY
  - X-Customer
  - X-Signature
  {% highlight ruby %}
  require 'net/http'
  ...

  service_url = "https://api.naver.com"
  uri = URI.parse(service_url)
  req = Net::HTTP::Get.new(uri)
  http = Net::HTTP.new(uri.host, uri.port)
  http.use_ssl = (uri.scheme == 'https')
  res = http.request(req)
  puts res.body
  {% endhighlight %}

- X-Signature은 Secret Key를 이용 아래 data를 sha256-hmac 으로 인코딩해야 한다.
  - `data` = `Milliseconds since Unix Epoch` + `'.'` + `http method` + `'.'` + `request_uri`
  {% highlight ruby %}
  require 'Base64'
  require 'openssl'
  ...

  ms_since_epoch = (Time.now.to_f * 1000).floor
  data = ms_since_epoch.to_s + '.' + http_method + '.' + api_path
  digest = OpenSSL::Digest::Digest.new('sha256')
  signature = Base64.encode64(OpenSSL::HMAC.digest(digest, secret_key, data)).strip
  {% endhighlight %}

- parameter를 추가해서 원하는 API 결과를 얻자.
  {% highlight ruby %}
  uri = URI.parse("https://api.naver.com/keywordstool")
  params = {showDetail: "1", hintKeywords: Keywords.join(",")}
  uri.query = URI.encode_www_form( params )
  req = Net::HTTP::Get.new(uri)
  {% endhighlight %}

- X-Timestamp의 값은 Signature 생성시 ms_since_epoch과 같은 값이어야 된다.
- (update)
  - 다른 mac에서 아래 에러가 난다.
    {% highlight text %}
    .../ruby/2.3.0/net/http.rb:933:in `connect_nonblock': SSL_connect returned=1 errno=0 state=error: certificate verify failed (OpenSSL::SSL::SSLError)
    {% endhighlight%}
  - 왜 이런지 모르겠다. [Googling 결과](http://mislav.net/2013/07/ruby-openssl/)는 state=SSLv3 인 에러에 대한 얘기다.
  - 우선은 아래와 같이 땜질을 해서 넘어가자.
     {% highlight ruby %}
      http.verify_mode = OpenSSL::SSL::VERIFY_NONE
     {% endhighlight %}

----
참고링크

- [Spotify verify webhook tutorial](https://help.shopify.com/api/tutorials/webhooks#verify-webhook)
- [Naver 검색광고 document](https://github.com/naver/searchad-apidoc)
