---
title: JQuery Event
layout: post
comments: true
category: [dev, jquery]
--- 



## Event 삭제 / unbind / off

    $(selector).off('event_name');


    $('.tab').on('click', function(){...});
    $('.tab').off('click');

    $('.tab').on('click.mynamespace', funcion(){...});
    $('.tab').off('click.mynamespace');

from. [stackoverflow][2]


## Hover event 삭제 / unbind / off

    $(selector).hover(function(){...});
    $(selector).off('mouseenter mouseleave')

from. [stackoverflow][1]







---

[1]: http://stackoverflow.com/questions/805133/how-do-i-unbind-hover-in-jquery
[2]: http://stackoverflow.com/questions/209029/best-way-to-remove-an-event-handler-in-jquery
