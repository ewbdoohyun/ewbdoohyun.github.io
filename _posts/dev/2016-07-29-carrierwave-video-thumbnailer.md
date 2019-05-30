---
layout: post
comments: true
title: Carrierwave video thumbnailer
category: [dev, rails]
---

video tag의 poster attribute에 동영상이 재생되기 전에 보이는 이미지를 정해 줄 수 있다.
w3school에서 poster attribute를 설정하지 않으면 동영상의 첫 frame이 사용된다고 했는데, Chrome에서는 이미지가 뜨지않았다.

[carrierwave-video-thumbnailer gem](https://github.com/evrone/carrierwave-video-thumbnailer)을 이용해서 간단히 poster attribute 용 이미지를 만들었다.

{% highlight ruby %}
class MediaUploader < CarrierWave::Uploader::Base
  include CarrierWave::Video::Thumbnailer

  version :thumb do
    process_extensions VIDEO_EXTENSIONS,
                        thumbnail: [{format: 'png',
                                    quality: 5,
                                    size: 400,
                                    strip: false,
                                    logger: Rails.logger}]

    def full_filename for_file
      png_name for_file, version_name
    end
  end
{% endhighlight %}

`thumb`이라는 버전으로 만들어 두었다. 쓸 때는 `<Model>.media.thumb.url` 으로 사용하면 된다.


* heroku에 ffmpegthumbnailer 설치 *
`carrierwave-video-thumbnailer gem`은 `ffmpegthumbnailer`을 이용한다.
Heroku로 배포시 buildpack 형태로 설치해줘야 한다.
buildpack은 [Heroku Element](https://elements.heroku.com/buildpacks/akomic/heroku-buildpack-ffmpegthumbnailer)에서 검색해서 사용하면 편리하다.


-----
<small>참고링크</small>

<small>[Heroku multi-buildpack 사용하기]({% post_url 2016-07-25-Heroku-multi-build-packs %})</small>





