---
title: 별점넣기 - Rails Gem Ratyrate
layout: post
comments: true
category: [dev, rails]
--- 


리뷰에 별점을 넣기 위해서 Ratyrate gem을 사용했다. 사실, ruby-toolbox에서 찾아보면 제일 많은 다운로드 수를 기록한 rating gem은 Ajaxful-rating이지만, Rails4에서 제대로 동작하지 않더라.([gibhub issue][1])

Google 검색에서 'Rails gem rating'을 쳤을 때 상위에 나오는 [Ratyrate][2]라는 gem을 사용하기로 했다. 우선 Ratyrate는 [jQuery Raty][3]라는 플러그인을 wrapping한 gem이다.


### 설치

    //Gemfile
    gem 'ratyrate'

    //Terminal
    rails g ratyrate user
    bin/rake db:migrate


### 사용

'dimension'을 설정하여 여러개의 별점을 줄 수 있습니다. [베베템][6] 에는 하나의 별점만 필요해 "all" 이라는 dimension을 사용했습니다.

    class Car < ActiveRecord::Base
        ratyrate_rateable "speed", "engine", "price"
    end


    class User < ActiveRecord::Base
        ratyrate_rater
    end


#### View Helper

    //@car의 'spped' dimension 대한 별점
    <%= rating_for @car, 'speed' %>

    //@car의 'speed' dimension 에 대한 특정 유저의 별점
    <%= rating_for_user @car, user, 'speed' %>

#### View Helper - Option

    //readOnly:
    <%= rating_for @car, 'speed', readonly: true %>


#### Model method

    // car의 "speed"에 rate object list
    //dimesion이 없으면, dimension을 설정하지 않은 rate만 반환한다.
    //전체 rate를 반환하는 기능은 이 gem엔 없다.
    car.rates "speed"

    car.raters "speed"


### 별모양 수정하기

*rating_for* 또는 *rating_for_user*에 Option을 사용해서 수정할 수 있다. 

gem에서는 starType 옵션을 제공하지 않지만, jquery raty plugin에서는 starType옵션을 제공합니다. 기본 starType은 <img>입니다.

font-awesome을 이용하기 위해서 *jquery.raty.js*에 starType을 <i>로 변경합니다.

    //jquery.raty.js
        _createStars: function() {
            ...
            // this.opt.path는 image의 path임.
            // attrs = { class: i, src: this.opt.path + this.opt[name] };
            attrs = { class: i, src: this.opt[name] };
            ...
            // $('<' + this.opt.starType + ' />', attrs).appendTo(this);
            $('<i></i>', attrs).appendTo(this);


    //_comment.html.erb

    <%= rating_for_user @item, comment.user, 'overall', readonly: true, star_on: "fa fa-star fa-1x", star_off: "fa fa-star-o fa-1x", star_half: "fa fa-star-half-o fa-1x" %>


### Cache 업데이트

*update_rate_average* method를 통해서 cache model의 record가 생성/수정된다.
Gem에서는 rate가 destroy될 때 이 method가 불리지 않는다. 이미 누군가 해결책을 [github issue로][5] 올렸지만, 아직 반여되진 않았다. 전반적으로 잘 관리가 안되고 있는 느낌이다.

cache인 *rating_caches* 는 gem에서 아래와 같이 선언되어 있다.

    dimensions.each do |dimension|
        has_one "#{dimension}_average".to_sym, -> { where dimension: dimension.to_s },
              as: :cacheable, 
              class_name: 'RatingCache',
              dependent: :destroy

*dependent: :destroy* 옵션이 있으니, Rate의 대상이 되는 객체를 삭제하면 RatingCache는 사라진다.

문제는 Rate가 삭제될 때, RatingCache가 업데이트가 되지 않는다.
아래와 같이 *update_rate_average*를 patch해서 사용하고 Rate가 삭제될 때 이 method를 불러주기로 했다.

    //config/ratyrate.rb
    module Ratyrate
     def update_rate_average(stars, dimension=nil)
        if average(dimension).nil?
          send("create_#{average_assoc_name(dimension)}!", { avg: stars, qty: 1, dimension: dimension })
        else
          a = average(dimension)
          a.qty = rates(dimension).count
          //start - 추가된 부분
          if a.qty == 0
            a.avg = 0
          else
            a.avg = rates(dimension).average(:stars)
          end
          //end
          a.save!(validate: false)
        end
      end
    end


    //Rate가 삭제되는 곳
    rate = Rate.find_by(rateable_id: self.id, rater_id: self.user.id)
    if rate
        rate.destroy
    end
    self.update_rate_average(0, "overall")


### Readonly 옵션이 제대로 안된다면...

아무리 readonly 옵션을 넣어도, 적용이 안되더라.
github에서 sourcecode를 봤는데, 전혀 이상할게 없었다.
실제 설치된 Gem 폴더를 열어 코드를 확인해보니, Github에서 보이는것과 달랐다.
버전을 확인해 보니 1.2.2alpha 버전이었다.

Gem폴더 열기 (SublimeText가 subl로 symbolic link되어 있을 때)

    //~/.bash_profile
    export BUNDLER_EDITOR=subl

    //Terminal
    bundle open ratyrate


Github에 나온 최신버전은 1.0.9이다.
그런데 Gemfile에 1.0.9로 넣으니 등록되지 않은 버전이었다.

    //Gemfile
    gem 'ratyrate', "1.0.9"



[rubygem.org][4]에 등록된 버전은 모두 1.2.0alpha 이후 버전이었다.
음.. 뭔가 이상하지만, github주소를 넣어서 1.0.9를 설치하기로 했다.

    //Gemfile
    gem 'ratyrate', 'git: git://github.com/wazery/ratyrate.git

github주소로 설치된 버전은 1.2.0alpha지만, 이후에 readonly부분의 bug가 수정되어서, readonly가 제대로 동작한다.

*git@*으로 시작하는 private link를 사용하면 Heroku에서 빌드가 안된다. 알아두자.



---


[1]: https://github.com/edgarjs/ajaxful-rating/issues/100
[2]: https://github.com/wazery/ratyrate
[3]: https://github.com/wbotelhos/raty
[4]: https://rubygems.org/gems/ratyrate/versions
[5]: https://gist.github.com/piyushbeli/8b92068ce72d28ed155a
[6]: http://bebetem.com
