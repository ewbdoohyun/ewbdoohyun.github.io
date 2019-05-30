---
title: Godaddy에서 Heroku로 DNS 설정하기
layout: post
comments: true
category: [dev, heroku]
--- 

1. Godaddy DNS 관리페이지에서 모든 레코드를 삭제.
2. 다음 레코드 추가
    - 유형 : CNAME
    - 이름 : www
    - 값 : your-heroku-app-name.herokuapp.com
    - TTL : 1시간
3. 포워딩 설정
    - 여기로 전달 : http://www.your_new_domain
    - 전달 유형 : 301 (영구)
    - 설정 : 포워딩만 가능
4. `Heroku dashboard > Settings` 에 Custom Domains에 추가
    - Domain name : www.your_new_domain.com


## SSL 설정시

2. 다음 레코드 추가
    - 유형 : CNAME
    - 이름 : www
    - 값 : your-doamin.herokudns.com
    - TTL : 1시간
3. 포워딩 설정
    - 여기로 전달 : https://www.your_new_domain
    - 전달 유형 : 301 (영구)
    - 설정 : 포워딩만 가능



---

all credit : [http://stackoverflow.com/questions/11492563/heroku-godaddy-send-naked-domain-to-www](http://stackoverflow.com/questions/11492563/heroku-godaddy-send-naked-domain-to-www)
