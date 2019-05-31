---
layout: post
comments: true
title:  "JavaScript Closure의 실 사용으로 보는 의미 ( 클로저 사용법 )"
category: [frontend, javascript]
---

클로저의 설명으로 내부함수가 외부 함수의 맥락에 접근 할 수 있는 것을 가르킨다고 많이들 설명한다.
하지만 단지 그 설명만으로 개인적으로 처음 클로저를 이해하는데 의아한 점이 많았으며, 좀 더 본인에게 와 닿는 설명을 위해 작성한다. ( 또한 내가 잊지 않기 위해)

아, 클로저라는 것이 그럴수도 있겠구나라고 생각은 들지만, 그래서 클로저를 왜 쓰고, 중요하며 어디에 좋은데? 라고 설명하기에는 너무 기능적인 측면에서 설명하지 않았나 싶다.

물론 결과적으로 하는 말이 다르지 않지만,  내부가 외부의 접근이 아니라 조금 다르게 표현을 해 보자.


```javascript
function makeAdder(x){
  var y = 1;
 	console.log(`X value = ${x}`);
  return function(z){
    y = 100;
    return x+y+z;
  };
};

var add5 = makeAdder(5);
var add10 = makeAdder(10);
//클로저에 x와 y의 환경이 저장됨

/// add5의 정의는 다음과 같이 되는 것이다.
// function(z){
// y = 100;
// //x 는 숨겨져 나타나지 앖음
//	return x + y + z;
// }

console.log(add5(2));  // 107 (x:5 + y:100 + z:2)
console.log(add10(2)); // 112 (x:10 + y:100 + z:2)
//함수 실행 시 클로저에 저장된 x, y값에 접근하여 값을 계산

```

위와 같은 상황에서 makeAdder(5) 의 리턴인 add5는 function(z) 를 가지고 있는 것이다. 다만! 이 와중에 외부 함수의 scope가 죽지 않고 살아 있는 것이며, 외부 함수에 접근할 수 있을 뿐이다. 정도면 되지 않을까.

그렇다면 설명이 조금 더 매끄럽게 이어질 수 있을 것 같다.

실용적으로 클로저는 다음과 같은 이점을 가질 수 있다.

1. 외부에서 접근이 불가능한 private variable 의 접근 (get method 처럼) 사용할 수 있다.
2. data를 control 하는 method를 연동시키기 좋다.



1 에 대한 것은… java의 encapsulation 개념을 보는 게 더 명확하게 이해 할 수 있고
2 에 대한 예제는 MDN을 그대로 따 왔다.

```javascript
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);

document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16;
```

끝

출처 https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures