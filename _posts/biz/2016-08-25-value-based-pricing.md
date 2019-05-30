---
layout: post
comments: true
title: 가치 기반 가격 (Value-based pricing)
category: [biz, pricing]
---


- 제조업인 경우엔 원가에 저절한 마진을 더해서 판매가가 결정된다.

- Software는 제조업의 가격결정구조가 잘 맞지 않는데,
  + 우선 원가 책정이 어려운데, 어느정도 개발을 하고 나면 한계비용이 0에 수렵하기 때문이다.
  + 또 기존에 없는 제품을 만드는 경우가 많기에, 벤치마크할 경쟁제품이 없는 경우가 많다.

- 제품의 가격을 설정하는 것은 매우 중요한 결정이다.
  + Startup은 고객의 관심을 끌거나 초기 사용에 대한 허들을 낮추기 위해 가격을 낮추는 경우가 많다
    + 대부분은 한 번 결정하고 다시는 바꾸지 않는다. `(BAD)`
  + 또는 충분한 구매의사가 있는 고객에게 해당 기능을 무료로 제공한다.

    `"가격을 한 번 결정하고 바꾸지 않으면, 뭔가가 잘못 된 것." Evernote CEO, Phiil Libin`
  
  + 가격을 결정하는 것은 한 번으로 끝나는 것이 아니라, 환경에 맞게 지속적으로 적절한 가격선(price point)를 찾아가는 과정이다.

- 대부분 원가(cost)와 판매가(price)의 Gap에 집중을 한다.
  + 이 gap은 판매자 입장의 판매동기이다.
  + 하지만 고객의 구매동기는 원가와 판매가의 Gap에서 만들어지지 않는다.
    * 판매가(price)와 고객이 받는 가치(percived value) 사이의 Gap이 고객이 구매하는 동기가 된다.    
    `"아무도 구매하지 않는다면, price와 percieved value 사이의 Gap이 작거나 아예 없는 것이다." - Michael Dearing`

  <center>
  <img src="https://embedwistia-a.akamaihd.net/deliveries/30f7495b541e5e179aa4f387a38b5ffa547fd0ee.jpg?image_crop_resized=640x360" alt="cost-price-percieved-value">
  from. <a href="https://www.sequoiacap.com/article/pricing-your-product">Sequoia</a>
  </center><br/>

  + Percieved value는 나라별로 다를 수 있다. 따라서 같은 price일 때, 각 나라의 유저별로 느끼는 구매동기가 다르다.
  + 판매가 줄어들면 판매가를 낮춰서 price와 percieved value 사이의 Gap을 늘리려는 노력을 한다.

- Perceived value를 수치로 표현한다.
  + 고객이 받는 가치는 세 가지 중 하나이다.
    * 더 우수하다.
    * 더 빠르다.
    * 더 저렴하다.
  + 고객의 현재 상태와 제품을 사용하는 미래를 비교해서 나타나는 숫자의 차이가 Perceived Value이다.
    + 개발기간을 X시간 만큼 단축
    + 판매수익 X달러 증가
    + 이익률 X퍼센트 감축

- Perceived value의 일정 비율을 price로 정해라.
  + 비율은 산업/경쟁수준에 따라 다르지만 대략 20% 정도가 적절
  + 80%는 낯선 제품을 구매해서 기존 업무 프로세스에 넣는 위험을 감수하는 고객의 몫으로 둬라.
  + 비율은 고객이 가져가는 risk에 따라서 달라진다.
    * 월 구독료 방식은 고객이 언제든 원할 때 취소가 가능하기에 선지급 모델보다 가격을 높게 책정할 수 있다.

    `"내 사업은 아주 간단하다. 고객은 2달러만 내면 10달러의 가치를 얻을 수 있다. 성공 비결은 따로 없다.” - Steve Walske, Parametric Technologies CEO`

- 제품을 똑같은 가치로 인식하는 고객은 없다.
  + 누구는 지금 가격이 적당하다고 느끼고 있지만, 누군가는 지금보다 10배는 더 지불해도 된다고 생각한다.
  + 고객이 어떻게 제품을 이용하는지부터 시작해, percieved value에 따라 고객 segemnt를 나눈다.
  + LinkedIn case
    * LinkedIn은 90% 유저가 자주 사용하지 않는 기능을 확인했다.
    * 이 기능을 이용하는 10%의 heavy 유저가 느끼는 가치는 90% 유저가 느끼는 가치보다 더 크다고 결론을 내리고, 2005년 이 기능을 premium 계정으로 분리해서 유료화했다.
    * Heavy 유저의 구매의사는 충분했고, 지난 4분기동안 $280M의 수익을 만들어냈다.

- 가격결정을 위한 가설 설정이 필요하다.
  + A/B Test를 통한 데이터도 중요하지만,
  + 데이터에만 의존해서 결정하지 않도록 한다.
    * 고객과 직원의 feedback
    * 경쟁사의 동향
    * 너의 직감, 모두를 활용하자.

    `가격을 정하는 것은 수학문제(math problem)가 아니다. 의사결정 문제(judgement problem)다. - Michael Dearing`

- Concorde case
  + 첫 6년간 적자
  + 마켓리서치에서 실제 탑승고객에게 질문을 했는데,
    * 대부분 비서들이 예약을 담당해서, 정확한 가격을 모르고 있었지만,
    * 탑승고객이 생각하기에 티켓 가격이 높을 것이라고 추측함.
  + 이 후 실제 고객이 지불할것이라고 생각하는 수준으로 티켓 가격을 조정 (이전 가격의 2배)
    * 높은 가격과 함께, 초 엘리트를 위한 서비스로 포지셔닝
  + 높은 가격에도 순이익 500M 파운드를 벌어들임

- 한국의 환경

    `한국에서 '가치'를 기준으로 가격을 정하는 것은 미국에 비해 상대적으로 어렵다. 특히 소프트웨어처럼 눈에 보이지 않는 제품에 대한 가치를 잘 인정하지 않고, 심하다 싶을 정도로 고객의 예산과 투자 의지에 모든 것을 맞추려는 경향이 있다. - MIT Startup Bible`


---

Reference

- [Concorde case](http://theadaptivemarketer.com/2012/01/14/a-pricing-lesson-from-the-concorde/)
- [Sequoia](https://www.sequoiacap.com/article/pricing-your-product)
- [MIT Startup Bible - (원제: Diciplined Entrepreeurship)](http://disciplinedentrepreneurship.com/)
- [How to price a SaaS solution](http://www.onthe.co/blog/how-to-price-a-saas-solution)
