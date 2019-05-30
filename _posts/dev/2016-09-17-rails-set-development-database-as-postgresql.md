---
title: Development DB를 Postresql로 세팅
layout: post
comments: true
category: [dev, rails]
--- 

pg install
- http://postgresapp.com/


pg user 확인
psql
\du : 유저 리스트
\l : database 리스트
\connect [database_name]
\dt : 연결된 database의 table 리스트

Gemfile
- gme 'pg'

datbase.yml
{% highlight ruby %}
development:
  adapter: postgresql
  encoding: unicode
  database: pnm_development
  pool: 5
  timeout: 5000
{% endhighlight %}

Console
- bin/rake db:create
or
- bin/rake db:reset


Heroku dump db를 local에 import
pg_restore --verbose --clean --no-acl --no-owner -h localhost -U myuser -d pnm_development latest.dump


https://devcenter.heroku.com/articles/heroku-postgres-import-export#restore-to-local-database
