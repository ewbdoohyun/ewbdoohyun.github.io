---
title: Javascript - Namespace 사용하기
layout: post
comments: true
category: [dev, javascript]
--- 


## Anti-Pattern : glbal variable

당연시 되어야 될 click event가 동자하지 않았다.
이유는 다른 파일이지만 같은 함수명을 사용하였기 때문!

모든 함수를 전역변수로 설정해서 사용했기에 일어나는 문제다.

    //a.js
    var add_to_checklist = function(){
        $('.footer__checklist__link').click( function(e) {
            e.preventDefault();
            console.log("A");
        });
    }

    //b.js
    var add_to_checklist = function(){
        $('.header__checklist__link').click( function(e) {
            e.preventDefault();
            console.log("B");
        });
    }

    // event handling
    add_to_checklist();     // not working.
                            // from a.js? from b.js?


## NameSpace

NameSpace를 설정하면, 같은 함수명을 사용해도 겹치지 않게 사용할 수 있다.
사실 Javascript 코드 양이 얼마 되지 않을 것 같아서 NameSpace를 사용하지 않았는데
이런 저런 기능들을 붙이다 보니 어느 덧 파일수도 늘어나고, 어떤 함수명을 썼는지도 기억하지 못하게 된 것.

    //a.js
    var footer = {
        add_to_checklist : function(){
            $('.footer__checklist__link').click( function(e) {
                e.preventDefault();
                console.log("A");
            });
        }
    }

    //b.js
    var header = {
        add_to_checklist : function(){
            $('.header__checklist__link').click( function(e) {
                e.preventDefault();
                console.log("B");
            });
        }
    }

    //event handling
    footer.add_to_checklist();      // from a.js
    header.add_to_checklist();      // from b.js



---


