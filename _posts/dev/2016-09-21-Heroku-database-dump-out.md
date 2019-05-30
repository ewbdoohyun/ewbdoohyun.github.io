---
title: Heroku database로 local database 세팅하기
layout: post
comments: true
category: [dev, rails, heroku]
--- 

## 0.Codes first

    heroku pg:backups capture
    curl -o latest.dump `heroku pg:backups public-url`
    bin/rake db:reset
    pg_restore --verbose --clean --no-acl --no-owner -h localhost -U myuser -d mydb latest.dump


## 1. Heroku version 확인
legacy heroku gem을 사용하고 있다면, gem을 제거하고 Heroku CLI를 다시 설치해야 합니다.

- legacy heroku gem 제거 : `gem uninstall heroku --all`
- Heroku toolbelk 설치 [https://devcenter.heroku.com/articles/heroku-command-line](https://devcenter.heroku.com/articles/heroku-command-line)



## 2. Heroku db dump-out

CLI를 이용한 방법 (heroku version: 3.43.12)

    heroku pg:backups capture

    curl -o latest.dump `heroku pg:backups public-url`


heroku.com을 이용한 방법

- visit [https://postgres.heroku.com/databases](https://postgres.heroku.com/databases)
- app의 database 선택
- PG Backups > Capture > Download

## 3. local database 생성/재생성

    bin/rake db:create

    또는

    bin/rake db:reset (기존 development db의 데이터는 삭제됩니다.)

## 4. Heroku backup db를 local db에 복구시키기

    pg_restore --verbose --clean --no-acl --no-owner -h localhost -U myuser -d mydb latest.dump
