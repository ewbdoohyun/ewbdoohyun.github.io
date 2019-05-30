---
title: Rails - libreadline 에러 해결하기
layout: post
comments: true
category: [dev, rails]
--- 

## 환경

rails 4.2.6
ruby 2.2.5

## 시작은 libreadline not found

아래 에러가 나면서 모든 rails command가 갑자기 먹통이 됨

    Users/sglee/.rbenv/versions/2.2.5/lib/ruby/gems/2.2.0/gems/activesupport-4.2.6/lib/active_support/dependencies.rb:274:in `require': dlopen(/Users/sglee/.rbenv/versions/2.2.5/lib/ruby/2.2.0/x86_64-darwin15/readline.bundle, 9): Library not loaded: /usr/local/opt/readline/lib/libreadline.6.2.dylib (LoadError)


## 알려진 해결책 #1. symbolic link

    ln -s /usr/local/opt/readline/lib/libreadline.7.0.dylib /usr/local/opt/readline/lib/libreadline.6.2.dylib

효과없음

## 알려진 해결책 #2. rb-readline


Gemfile

    group :development do
        gem 'rb-readline'
    end


효과없음

## 알려진 해결책 #3. readline 설치 & ruby 재설치

    brew install readline ruby-build
    rbenv install 2.2.5
    gem prinstine -all


효과없음


## 내가 해결한 방법

git repository를 다시 cloning

    mv bebetem bebetem_old
    git clone <git repository> bebetem


의심되는 원인이나 확인은 안됨
- pow
    + pow를 제거했느나 똑같은 현상이 발생


---


[알려진 해결책 #3 - mikekusold.com][2]

[2]: https://mikekusold.com/development/ruby-on-rails-console-wont-start.html
