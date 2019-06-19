---
layout: post
title:  "Slim Framework"
date:   2019-05-19 13:00:00
author: Danny Kim
categories: Backend PHP Slim framework
tags:	php slim
---
Slim Framework 적용기 (간단한 Restful API을 PHP로 )

![1560917521622](C:\Users\trumpia\AppData\Roaming\Typora\typora-user-images\1560917521622.png)

다음과 같은 형태로 Ajax 콜을 PHP로 관리 하고 있었다.

 생각을 해 보니, 단순 콜 하나일 때는 상관이 없었으나, 여러개가 되고, 담당자가 변경이 된다고 하였을 때 유지보수나, 새로운 기능을 만든다고 생각을 했을 때 일관성을 유지하기가 힘들 것 같았다.

해당 부분을 Restful API로 개조하여 사용하기로 결정하고, 어떤 Framework을 사용하는 것이 좋을 지에 대하여 고려해 보았다.



고려한 점은 다음과 같다.

1. 적은 러닝 커브를 지닐 것 ( 즉 사용하는데 걸리는 시간이 적게 걸릴 것)

2. 어느정도 인정을 받은 Framework일 것

   [참고1](https://nordicapis.com/5-lightweight-php-frameworks-build-rest-apis/)

   [참고2](<https://www.slant.co/topics/6956/~php-frameworks-for-building-a-restful-api>)

   를 따라 

**라라벨 ( Lalavel )** 

1. 본인을 비롯한 나머지 개발자 모두 라라벨을 사용한 적이 없어, 러닝 커브가 존재
   (그렇다고 다른 서비스들에 라라벨을 추가로 사용할 것 같지도 않음)
2. 단순히 저 File Cache 기능하나만을 위해 라라벨을 공부해야 한다는 점이 시간대비 좋지 못함.
3. 다른 기능들을 사용할 예정이 전혀 없기 때문에 상대적으로 무겁다고 생각함.

을 제외하고, 최종적으로 Slim을 선택하게 되었다.

[Slim Framework](slimframework.com) - a micro framework for php

Slim은 주용한 몇 가지 기능만 담은 매우 Simple한 Framework이다.

HttpRequest를 받고, 적절한 Callback 을 호출하여 HTTP Response를 리턴하는 데 그 의의가 있다.

이게 전부다!

Data를 Consume, 또는 변형(repurpose), publishing 하는데 목적을 두고 있으며, 빠르게 Prototype을 생성할 수 있다고 설명 되어 있다. 실제로도 매우 간단한 코드만으로도 구현이 가능하다.

가벼운 것이  제일 마음에 들었고, 사용법 또한 간단하다. 



```php
$app->group('/api-group',function() use ($app){

    $app->get('',function ($request, $response, $args){
        $params = $request->getQueryParams();
        /// 실 처리 부
        return $response->write(" write return ");
        }else{
          $failJson = array("result"=>"false","message"=>"needed parameter -> domain");
            return $response->write(json_encode($failJson));
        }
    });
       $app->get('/all',function ($request, $response, $args){

        return $response->write(" write return ");
    });
})->add(function($request,$response,$next){
    $response = $next($request, $response);
    return $response;
});
```



다음과 같이 api를 group 단위로 관리 할 수 있다. (  예제에는 get 하나로 된 것도 존재한다.)

다만 call이 많아질 경우를 대비하여 Group 단위로 콜을 관리 하면 좋을 듯 싶다.