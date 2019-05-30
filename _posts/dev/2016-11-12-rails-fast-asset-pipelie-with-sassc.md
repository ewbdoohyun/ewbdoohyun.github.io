---
title: Rails - dev 모드에서 sass 컴파일 속도 높이기
layout: post
comments: true
category: [dev, rails]
---


프론트 작업할 때, sass 파일을 수정하고 브라우저에서 확인하는 것을 반복할 경우가 많다.
'sass-rails' gem을 사용하고 있다면, 'sassc-rails' gem으로 바꾸는 것 만으로 이 작업의 속도가 훨신 좋아진다!

    //Gemfile
    gem 'sassc-rails'



----

Full credit : [http://marianposaceanu.com/articles/making-rails-asset-pipeline-faster](http://marianposaceanu.com/articles/making-rails-asset-pipeline-faster)
