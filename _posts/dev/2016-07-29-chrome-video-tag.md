---
layout: post
comments: true
title: Chrome에서 video tag 사용
category: [dev, web]
---

Chrome limits the number of videos/audio files to 4 or 6 (I can't remember which) so, as brianchirls mentions, you need to set the preload attribute, although you need to set it to none.

My recommendation is to provide a poster image (via the poster attribute) for all your videos and to set preload="none" to all of them. That way the browser only actually tries to load them if the user actively clicks the play button.

Alternatively you could set the first 4/6 videos with preload="metadata" and the rest to preload="none".
