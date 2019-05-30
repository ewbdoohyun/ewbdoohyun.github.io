

### Default 값 설정
유저 브라우저 값을 사용하는 경우엔 default 값을 설정해준다.


    session[:last_product_page] = request.env['HTTP_REFERER'] || products_url

<small>from [http://railscasts.com/episodes/131-going-back?autoplay=true][1]</small>




---

[1]: http://railscasts.com/episodes/131-going-back?autoplay=true