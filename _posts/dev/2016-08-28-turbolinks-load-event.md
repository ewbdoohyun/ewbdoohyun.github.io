---
title: document ready & Turbolinks
layout: post
comments: true
category: [dev, rails]
---

- turbolink 5 기준이다.
    - `$(document).ready` 를 `$(document).on("turbolinks:load",` 로 바꿔주면 된다.
    - turbolink github에 관련 내용 [링크][turbolink-github]
- [railscast][railscast]를 보니 이전 버전에서는 `$(document).on("page:load",` 를 추가해야 되었나보다.


[turbolink-github]: https://github.com/turbolinks/turbolinks#running-javascript-when-a-page-loads
[railscast]: http://railscasts.com/episodes/390-turbolinks
