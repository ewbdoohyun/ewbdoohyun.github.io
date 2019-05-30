---
layout: post
comments: true
title: EXIF 옵션에 따라 이미지 회전시키기
categories: [dev, rails]
---

모바일에서 세로로 찍은 사진을 [ara house](https://ara-house.herokuapp.com)에 올리니, 데스크탑에서 제대로 보이지 않았다.
모바일에서는 브라우저가 자동으로 이미지를 회전시켜주지만, 데스크탑에서는 그렇지 않은가보다.

[rails tutorial](https://www.railstutorial.org/)를 충실히 따라한 ara house는 이미지 작업을 ```Carrierwave gem```을 이용해서 구현했다.

{% highlight ruby %}
class PictureUploader < CarrierWave::Uploader::Base
  include CarrierWave::MiniMagick
  ...
  process :auto_orient
  def auto_orient
    manipulate! do |image|
      image.tap(&:auto_orient)
    end
  end
{% endhighlight %}

`process`를 이용해서 이미지가 업로드 될 때 callback method를 걸어둘 수 있다.

`manipulate!`는 이미지 프로세싱에 사용하는 MiniMagick command를 사용할 수 있게 해준다.

`image.tap(&:auto_orient)`는 MiniMagick의 `-auto_orient` command을 실행시킨다. 이 command가 EXIF에 따라서 적절히 이미지를 회전시켜주는 고마운 아이이다.

`.tap`은 object를 단순히 block으로 넘기고, 결과값을 다시 return해주는 method이다.

`def auto_orient`를 아래와 같이 module에 method를 추가하는 형태로 하고 싶었는데, 파일을 어디에 둬야될지 몰라서 `PictureUploader`에 넣어뒀다.

아는 분 계시면 좀 알려주세요.

{% highlight ruby %}
module CarrierWave
  module MiniMagick
    def auto_orient
      manipulate! do |image|
        image.tap(&:auto_orient)
      end
    end
  end
end
{% endhighlight %}



-----
<small>참고링크</small>

<small>[MiniMagick auto_orient method](http://www.imagemagick.org/script/command-line-options.php#auto-orient)</small>

<small>[Carrierwave auto_orient](https://gist.github.com/tanraya/7438337)</small>

