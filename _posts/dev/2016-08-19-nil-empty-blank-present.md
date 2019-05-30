---
layout: post
comments: true
title: nil?, empty?, blank? 그리고 present?
category: [dev, rails]
---

- `item.comment`가 존재하는지를 `.empty?`를 통해 확인했다.
- `item.comment`가 `nil`인 경우에 exception이 발생한다.
- nil?, empty?는 ruby에서 제공한다.
- blank?, present?는 rails에서 제공한다.
- 자세한 내용은 [stackoverflow](http://stackoverflow.com/questions/885414/a-concise-explanation-of-nil-v-empty-v-blank-in-ruby-on-rails)를 참고.
