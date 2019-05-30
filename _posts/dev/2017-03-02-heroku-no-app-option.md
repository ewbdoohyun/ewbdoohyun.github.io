---
title: Heroku - app option을 사용하라고 error 발생시
layout: post
comments: true
category: [dev, heroku]
--- 

heroku cli를 세팅하는 경우에, 다음과 같은 error가 나올 때가 있음.

    Run this command from inside an app folder or specify which app to use with --app APP


heroku cli는 git remote로 등록된 heroku git repository 주소를 보고 어떤 app인지를 판단함.

    > git remote
    origin


머신을 새로 세팅하는 경우 remote에 heroku가 없어서 app을 지정해 달라는 error를 발생시킴
다음과 같이 remote repository를 등록하면 더 이상 `-app` 옵션이 필요없다.

    > git remote add heroku YOUR_HEROKU_REPOSITORY
    > git remote
    origin
    remote


