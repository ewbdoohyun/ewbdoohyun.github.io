

http://eloquentjavascript.net/09_regexp.html

    var match = /\d+/.exec("one two 100");
    console.log(match);
    // → ["100"]
    console.log(match.index);
    // → 8


variable로 부터 regex 만들기


http://stackoverflow.com/questions/494035/how-do-you-pass-a-variable-to-a-regular-expression-javascript

    var pattern = "regex_pattern"
    var re = new RegExp(pattern,"g");