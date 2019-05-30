---
title: .xyz 때문에 짜증이 나신다면 - GA에서 스팸제거하기
layout: post
comments: true
category: [dev, google analytics]
--- 


### Ghost Spam

주로 referral report에 나타나는 스팸성 트래픽은 랜덤하게 Google anlaytics(GA) ID를 만들어서 트래픽을 보낸다.
주요 목적은 GA 담당자가 보고서에 나온 트래픽을 보고 해당 사이트에 방문하도록 만드는 것이다.

스패머는 랜덤하게 만들어진 GA ID를 사용하기 때문에, 해당 ID를 사용하는 사이트가 어떤 사이트인지 모른다.
그래서 hostname이 정해져있지 않거나, 유명한 사이트의 도메인으로 채워넣곤 한다.
Referral report에서 2nd dimension을 hostname으로 놓고 보면, 스팸여부를 확인할 수 있다.

![구글 애널리틱스 추천 리포트 고스트 스팸 (google analytics referral report - ghost spam)](/public/2016-12-19/xyz_spam.png)

{: .center}
[결국 모든 referral 트래픽이 다 스팸 트래픽..]

<br>

### Filter to prevent ghost spam

whitelist 형태의 filter를 이용해서 유효한 hostname을 가진 트래픽만 GA에서 받아들이도록 하면
하나의 filter로 ghost spam traffic을 모두 차단 할 수 있다.

`Audience > Technology > Network` 에서 Primary dimension을 hostname으로 설정하면, GA로 들어온 모든 트래픽의 hostname을 확인할 수 있다.

여기서 자기 소유의 도메인과 관련된 hostname, 결제관련 서비스의 hostname, 번역서비스의 hostname 등을 확인한다.

![구글 애널리틱스 호스트명 확인 (google analytics check host name)](/public/2016-12-19/hostname.png)

<br>

이제 필터를 만들 차례다.

`Admin > Filters > Add filter`

Custom을 선택하고, Include를 선택한다. 앞서 말했듯이 이 필터는 whitelist필터이다. 허용한 traffic만 들어온다. 오타에 주의하자.

Filter field를 hostname으로 설정하고

아래 filter pattern에 앞서 확인한 hostname을 적어준다.

![구글 애널리틱스 스팸 필터 - Google analytics ghost spam filter](/public/2016-12-19/filter.png)



만약 확인한 hostname이 "www.example.com", "example.com" 이었다면 아래와 같이 정규표현식으로 적어주어야 한다.

{% raw %}
    www\.example\.com|example\.com
{% endraw %}

hostname사이에 {% raw %} | {% endraw %}이 표시를 꼭 넣어줘야 한다. 
CS쪽 지식이 있는 사람이라면 단번에 or 연산자라는 걸 알것이다.

주의해야 할 것은 hostname사이에 빈칸이 들어가서는 안된다. 다른 hostname으로 인식하더라.






---

[reference - moz blog][1]


[1]: https://moz.com/blog/stop-ghost-spam-in-google-analytics-with-one-filter
