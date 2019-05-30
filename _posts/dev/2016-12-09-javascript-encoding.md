---
title: Javascript Unicdoe/UTF-8 인코딩
layout: post
comments: true
category: [dev, javascript] 
--- 


### Unicode

    String.prototype.toUnicode = function(){
        var result = "";
        for(var i = 0; i < this.length; i++){
            result += "\\u" + ("000" + this[i].charCodeAt(0).toString(16)).substr(-4);
        }
        return result;
    };

    var output = input.toUnicode();

### UTF-8

    var output = encodeURI(input);


---

[stackoverflow][1]

[1]:http://stackoverflow.com/questions/21014476/javascript-convert-unicode-string-to-javascript-escape
