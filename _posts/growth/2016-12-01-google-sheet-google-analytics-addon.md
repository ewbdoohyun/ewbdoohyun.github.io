---
title: Google Sheet에서 Google Analytics add-on 사용하기
layout: post
comments: true
category: [growth, Measure]
--- 

[Google analytic add-on][1] 사용시 주의점 간략히 정리해 봅니다.

<br>

## 1. 빈줄금지

모든 셀에 아래와 같이 빈줄이 있으면 안됩니다.

<img src="/public/google-analytics-add-on-no-blank.png" alt="google-analytics-addon-no-blank" style="width:auto"/>


<br>

## 2. Filter

filter의 operator는 string형태로 바로 사용하면 됩니다. (==, >, <)
URL 버전(%3D%3D, %3E, %3C)으로 사용하면 동작하지 않습니다.

Google sheet에서는 equal operator가 `=` 이지만, google anlaytics addon에서는 `==` 입니다.
[filter operator list][5]

아래와 같이 operator와 dimension사이에 빈칸있으면 안됩니다.

<img src="/public/google-analytics-add-on-no-space.png" alt="google-analytics-addon-no-spac" style="width:auto">

filter를 AND조건으로 붙일 때는 filter 사이에 ';'(세미콜론)을 붙입니다.

`ga:channelGrouping==Referral;ga:sessions>10`

문자열을 따움표로 감싸지 않아야 합니다.

`ga:keyword!=(not set)`

<br>

## 3. Sort

내림차순 정렬은 정렬기준 앞에 '-'를 붙입니다.

`-ga:sessions           //방문자수 내림차순 정렬`



---
[Query Test][2]

[Dimension/metric explorer][3]

[Google Analytics AddOn][4]


[1]: https://developers.google.com/analytics/solutions/google-analytics-spreadsheet-add-on
[2]: https://ga-dev-tools.appspot.com/query-explorer/
[3]: https://developers.google.com/analytics/devguides/reporting/core/dimsmets
[4]: https://developers.google.com/analytics/solutions/google-analytics-spreadsheet-add-on
[5]: https://developers.google.com/analytics/devguides/reporting/core/v3/reference#filters
