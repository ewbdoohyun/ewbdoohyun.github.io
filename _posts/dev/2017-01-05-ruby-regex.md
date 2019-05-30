---
title: Ruby Regex
layout: post
comments: true
category: [dev, ruby]
--- 


regex를 사용하기 전에, 특정 문자열을 바꾸는 경우라면, 간단히 아래 함수를 사용할 수도 있다.


.sub

첫 번째 matched element만 변경

    "123 1231 123".sub(" ","")       //"1231231 123"

 
.gsub

모든 matched elements를 변경

    "123 1231 123".gsub(" ","")       //"1231231123"




## Pattern matching

regex 패턴 매칭 여부만 알면 된다면, =~ operation으로 가증
pattern이 발견되면 패턴이 시작되는 character의 index를 반환하고
pattern이 없다면 nil을 반환한다.

    /123/ =~ '12345'    // 0
    /123/ =~ '012345'    // 1
    /123/ =~ '2345'    // nil


## [match][1]



[1]: https://ruby-doc.org/core-2.2.0/Regexp.html
