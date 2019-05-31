---
layout: post
comments: true
title:  "[Oracle] Materialized View의 Exception ( ORA-12034 )"
category: [database, oracle]
---

일단 Materialized View에 대하여 짧게 언급하고 지나가겠습니다.
Materialized View는 일반 View와는 다르게, 물리적인 데이터를 실제로 가지고 있는 View입니다.
일반적으로 우리가 생각하고 있는 View는 물리적으로 존재하는 데이터가 아닙니다. 하나 이상의 테이블의 결과를 논리적인 형태로 풀어놓은 것에 불과한 것이죠.
다만, 쿼리의 사용 양, 매번 Select해 오기에는 부하가 클 경우 실제 데이터로 만들어, 이를 사용하도록 하는 것을 Materialized View 라 합니다. 이것에 대한 갱신 주기, 방식 등등을 정하여 좀 더 쾌적한 사용을 할 수 있도록 해 주는데 있습니다.
좀 더 상세한 설명은 http://www.gurubee.net/lecture/1857 를 참조하시면 될 듯 싶습니다.
(차후 업데이트 예정)
—
현 운영 중인 곳에서 구체화된 뷰 에서 자동으로 새로고침이 되지 못하는 문제가 발생하였습니다.
현재 운영중인 Database에서는 특정 시간별로 FAST 방식을 통해 일정 시간마다 새로 고침을 해 주고 있는데요.

![image](https://user-images.githubusercontent.com/42507943/55311240-59850800-549d-11e9-80b2-12d69fb0c22b.png)

모종의 이유로  로그의 timeline이 꼬였다고 하고 있는데.

![image](https://user-images.githubusercontent.com/42507943/55311337-9bae4980-549d-11e9-9980-2c38aeca40d5.png)

현재 사이트는 Fast 방식으로 운영하고 있습니다.
 새로 고침을 스케줄링과 관련없이 새롭게 시작하려고 하니

![image](https://user-images.githubusercontent.com/42507943/55311343-9fda6700-549d-11e9-87bf-aee188dce64b.png)

한글화가 되어 있지만, 내용상 보았을 떄 Fast와 Complete 옵션만 적용 한 것으로 나와 있습니다.
FAST로는 해당 증상의 해결이 어렵습니다.
( Timeline이 꼬여있기 떄문에, Fast 옵션으로는 해당증상을 해결할 수 없다. 변경된 일부만 시도되기 때문에.)
Complete로 다시 돌린 후 원래대로 Fast로 돌아가도록 해 둡니다.

stack overflow에서”Complete Refresh” means you truncate entire materialized view and insert new data. 라는 표현을 사용하는데, 이보다 적절한 한글말을 찾지 못해 본문 그대로 인용합니다.
Fast 방법이 문제가 생길 수 있는 환경이라면 Fast + 실패시 Complete로 돌 수 있는 Force로 설정해 두는 것도 좋은 방법입니다. ( 문제가 되었던 곳은 개발서버 쪽이였기 때문에, 증상 확인을 위해 그대로 Fast로 사용하고 있습니다.)

참조한 곳들.
http://www.gurubee.net/lecture/1857
https://stackoverflow.com/questions/41465445/what-is-the-difference-between-complete-refresh-and-fast-refresh-in-materialized
https://docs.oracle.com/database/121/DWHSG/basicmv.htm#DWHSG-GUID-A7AE8E5D-68A5-4519-81EB-252EAAF0ADFF
