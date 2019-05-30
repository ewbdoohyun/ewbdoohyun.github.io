---
layout: post
comments: true
title: 빠른 테스트 - Guard & Zeus
category: [dev, rails]
---

### [Zeus][1] & [Guard][2]

zeus는 rails app을 미리 로딩해 두어서, test 실행 마다 로딩에 걸리는 시간을 줄일 수 있습니다.

guard는 파일이 변경되는 것을 인지해서, 변경된 spec 파일의 test suits만 실행시켜줍니다.

guard 설정은 [테스트 자동화 - guard]({% post_url 2016-08-20-rails-guard%})를 참고하세요.

`Gemfile`

    group :development, :test do
      gem 'zeus'
    end

`Zeus 설치 & 시작`

    zeus start

`GuardFile 수정`

    guard :rspec, cmd: "zeus rspec" do
    ...

`Guard 시작`

    guard



----

[RailsCasts][3]

[1]: https://github.com/burke/zeus
[2]: https://github.com/guard/guard
[3]: http://railscasts.com/episodes/413-fast-tests
