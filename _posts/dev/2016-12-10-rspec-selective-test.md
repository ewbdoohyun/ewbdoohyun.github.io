---
title: Rspec - 원하는 테스트만 실행하기
layout: post
comments: true
category: [dev, rspec]          
--- 

spec_helper.rb

    config.filter_run_when_matching :focus

- `it`, `describe`, `context` 뒤에 :focus를 붙이면 해당 test만 실행
- :focus가 붙은 text가 하나도 없으면, 모든 test를 실행


---
