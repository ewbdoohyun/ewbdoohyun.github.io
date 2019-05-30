---
layout: post
comments: true
title: SQL for Cohort analysis
category: [growth, cohort]
---

- 가입일로 보는 cohort는 크게 의미가 없기에, 특정 이벤트 액션별로 비교한다.
  {% highlight sql %}
  select activity.date,
        count(distinct activity.user_id) as active_users,
        count(distinct future_activity.user_id) as retained_users,
        count(distinct future_activity.user_id) /
        count(distinct activity.user_id)::float as retention
  from activity
  left join activity as future_activity
        on activity.user_id = future_activity.user_id
           and activity.date = future_activity.date - interval '1 day'
  group by 1
  {% endhighlight %}



[Periscope blog](https://www.periscopedata.com/blog/how-to-calculate-cohort-retention-in-sql.html)
[sangkon blog](http://www.sangkon.com/2015/11/21/using_sql_for_cohort/)
