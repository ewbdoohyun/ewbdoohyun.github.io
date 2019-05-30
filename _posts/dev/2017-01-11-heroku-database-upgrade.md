---
title: Heroku database 업그레이드
layout: post
comments: true
category: [dev, heroku]
--- 

무료버전인 Hobby-Dev에서 $9인 Hobby-Basic 버전으로 변경하는 경우

<br>

### 우선 백업부터.

    heroku pg:backups capture

<br>

### 코드.


    heroku pg:info

        === HEROKU_POSTGRESQL_XXXXXX_URL
        Plan:        Hobby-dev

    heroku addons:create heroku-postgresql:hobby-basic

        Created postgresql-xxxxxx-xxxxx as HEROKU_POSTGRESQL_AQUA_URL

    heroku maintenance:on
    heroku pg:copy HEROKU_POSTGRESQL_XXXXXX_URL HEROKU_POSTGRESQL_AQUA_URL --app your_app_name
    heroku pg:promote HEROKU_POSTGRESQL_AQUA_URL
    heroku maintenance:off


<br>

### 설명.

a. 사용중인 database url 확인

    heroku pg:info

        === HEROKU_POSTGRESQL_XXXXXX_URL
        Plan:        Hobby-dev

<br>
 
b. 새로운 postgresql add-on 설치

    heroku addons:create heroku-postgresql:hobby-basic

        Created postgresql-xxxxxx-xxxxx as HEROKU_POSTGRESQL_AQUA_URL

<br>

c. 데이터베이스 복사 전에 서버점검 모드로 변환

    heroku maintenance:on


<br>

d. 이전 데이터베이스를 새로운 데이터베이스로 복사
heroku에서 database 생성시, 긴 url 대신에 alias를 제공.

    heroku pg:copy HEROKU_POSTGRESQL_XXXXXX_URL HEROKU_POSTGRESQL_AQUA_URL --app your_app_name


<br>

e. 새로운 데이터베이스를 주 사용 데이터베이스로 선택

    heroku pg:promote HEROKU_POSTGRESQL_AQUA_URL


<br>

f. 서버점검 모드 해제

    heroku maintenance:off




---

[Upgrading Heroku Postgres Databases
][1]


[1]: https://devcenter.heroku.com/articles/upgrading-heroku-postgres-databases
