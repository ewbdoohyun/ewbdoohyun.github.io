---
title: Heroku - Deploy시에 자동으로 migration 하기
layout: post
comments: true
category: [dev, rails, heroku]
--- 

Profile 을 root directory에 추가

Procfile
    
    release: bundle exec rake db:migrate

---

all credit [aspiringwebdev.com][1]
[1]: http://aspiringwebdev.com/run-rails-migrations-automatically-on-heroku/
