---
title: Integration test or Unit test
layout: post
comments: true
category: [dev, rails, test]
--- 

Integration_test에서는 사용자가 신경 쓸만한 부분을 테스트한다.

- 메일을 받는다
- 메일 주소를 넣는다.

사용자가 신경쓰지 않는 부분들은 Unit test 레벨에 넘긴다.

- 메일에 올바른 password_reset token을 받는다.



---
[railscast][1]

[1]: http://railscasts.com/episodes/275-how-i-test
