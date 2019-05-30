---
title: Sublime Package
layout: post
comments: true
category: [dev, sublimetext]
---

`Jekyll`

단순히 highlight snippet 때문에 사용. (ST2는 지원하지 않는다.)

`MarkdownEditing`

- markdown tag (`, ** 등등) 둘러 쌓인 부분이 highlight된다.
- link같은 경우엔 link name이 강조되고, 실제 url 부분이 dim처리 된다.
- markdown으로 editing 할 때 훨씬 가독성이 좋아진다.

`SublimeERB`

- ruby or rails 개발 시 필수
- <% %> erb tag를 단축키(command + shift + .)를 이용해서 만들 수 있게 해준다.

keybinding

    [
        { "keys": ["command+shift+."], "command": "erb" }
    ]




`rails partial`

- View 파일의 특정부분을 선택하고 alt + p 를 누르면 partial file을 만들 수 있다.

`SideBarEnhancements`
- 필수 package
- sublime sidebar에서 파일 이동이 가능해집니다! (ST2는 지원하지 않음)
