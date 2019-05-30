---
layout: post
comments: true
title: Heroku에 ffmpeg 설치하기 (Multi buildpack 사용하기)
category: [dev, heroku]
---


Video 파일 변환을 위해서 `carrierwave-video` gem을 사용했다. 이 gem은 ffmpeg를 사용하기 때문에, Heroku에도 ffmpeg를 설치해 주어야 한다.

carrierwave-video는 audio codec으로 fdk-aac를 사용한다. License 문제로 fdk-aac를 포함한 binary file을 공식적으로 배포하지는 않고, 직접 compile해서 사용해야 한다.

ffmpeg 3.0 부터는 audio codec이 안정화 되었다고 하니, 기본 audio codec인 `aac`를 사용하기로 했다.

{% highlight ruby %}
  process_extensions VIDEO_EXTENSIONS, encode_video: [:mp4, audio_codec: "aac"]
{% endhighlight %}

Heroku에 ffmpeg를 설치하기 위해서는 buildpack을 이용해야 한다.
여러개의 buildpack을 이젠 deprecated되었지만, 아직 잘 동작하는 ddollar의 [heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi)를 사용했다.

.buildpacks에 buildpack 정보를 넣는다. buildpack은 직접 생성할 수도 있고, [Heroku element](https://elements.heroku.com/buildpacks)에서 다른 사람이 만든 buildpack을 검색할 수도 있다.


{% highlight text %}
  <.buildpacks>
    https://github.com/jonathanong/heroku-buildpack-ffmpeg-latest.git
{% endhighlight %}

heroku에 ddollar의 buildpack을 추가한다.
{% highlight text %}
  > heroku buildpacks:add https://github.com/ddollar/heroku-buildpack-multi
{% endhighlight %}

{% highlight text %}
  > git push heroku master
{% endhighlight %}
