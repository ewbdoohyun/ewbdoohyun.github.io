---
title: Heroku에서 Timezone 설정하기
layout: post
comments: true
category: [dev, heroku]
--- 

### 0. Codes

    heroku config:add TZ="Asia/Seoul"

[tz database timezone][2]


---
full credit - [http://blog.pardner.com/2012/08/setting-the-default-time-zone-for-a-heroku-app/][1]

[1]: http://blog.pardner.com/2012/08/setting-the-default-time-zone-for-a-heroku-app/
[2]: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
