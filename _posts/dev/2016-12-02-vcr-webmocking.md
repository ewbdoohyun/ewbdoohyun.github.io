---
title: VCR - 빠른 3rd party API 테스트 
layout: post
comments: true
category: [dev, rails]
--- 

<br>

### :vcr 태그 사용 

`/spec/support/vcr.rb`

    VCR.configure do |config|
      config.cassette_library_dir = "spec/vcr"
      config.hook_into :webmock
      config.ignore_localhost = true

      # :vcr tag 사용
      config.configure_rspec_metadata!
    end


<br>

### :vcr 태그에 option 전달

    context 'with valid attributes', :vcr do

    context 'with valid attributes', vcr: {record: :new_episodes} do



### VCR::Errors::UnhandledHTTPRequestError

- 카세트에 저장된 URL과 test에서 요청하는 URL이 다를 때 발생
- Factory_girl에서 sequence를 이용해 URL을 만들 경우, 이후에 테스트 수트가 추가되면서 URL이 변경되는 경우 발생
- create() method에 URL관련된 attribute를 parameter로 넘겨줘서 고정시켜 해결 [vcr-issue][3]


---

[vcr reference][2]


[1]: https://github.com/vcr/vcr
[2]: https://www.relishapp.com/vcr/vcr/v/2-2-5/docs/test-frameworks/usage-with-rspec-metadata
[3]: https://github.com/vcr/vcr/issues/554

https://semaphoreci.com/community/tutorials/stubbing-external-services-in-rails

http://marnen.github.io/webmock-presentation/webmock.html

http://railscasts.com/episodes/291-testing-with-vcr
