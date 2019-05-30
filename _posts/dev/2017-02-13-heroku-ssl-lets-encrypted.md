---
title: Heroku SSL 적용 - Let's encrypt CA
layout: post
comments: true
category: [dev, heroku]
--- 


## 무료 SSL certificate

[let's encrypt][1] 에서 무료 SSL certificate 을 발급받을 수 있다.


## SSL certificate 발급

`certbot` 을 설치.

    brew install certbot


`manual` 방식으로 cerbot을 실행
이 때 `-d` 옵션으로 도메인을 지정할 수 있다.
기본도메인과 함꼐 `www` hostname에 대해서도 함께 추가해야, 웹 브라우저에서 비공개 커넥션 에러를 방지 할 수 있다. 


    sudo certbot certonly --manual -d bebetem.com -d www.bebetem.com
    


로그들

    Saving debug log to /var/log/letsencrypt/letsencrypt.log
    -------------------------------------------------------------------------------
    You have an existing certificate that contains a portion of the domains you
    requested (ref: /etc/letsencrypt/renewal/bebetem.com.conf)

    It contains these names: bebetem.com

    You requested these names for the new certificate: bebetem.com, www.bebetem.com.

    Do you want to expand and replace this existing certificate with the new
    certificate?
    -------------------------------------------------------------------------------
    (E)xpand/(C)ancel: E
    Renewing an existing certificate
    Performing the following challenges:
    http-01 challenge for bebetem.com
    http-01 challenge for www.bebetem.com

    -------------------------------------------------------------------------------
    NOTE: The IP of this machine will be publicly logged as having requested this
    certificate. If you're running certbot in manual mode on a machine that is not
    your server, please ensure you're okay with that.

    Are you OK with your IP being logged?
    -------------------------------------------------------------------------------
    (Y)es/(N)o: Y

    -------------------------------------------------------------------------------
    Make sure your web server displays the following content at
    http://bebetem.com/.well-known/acme-challenge/R9CHAx3oXTzw0NhYX-YMM2cKgP4hErSy92CUO4HUSRE before continuing:

    R9CHAx3oXTzw0NhYX-YMM2cKgP4hErSy92CUO4HUSRE.YOUR_RANDOM_KEY

    If you don't have HTTP server configured, you can run the following
    command on the target server (as root):

    mkdir -p /tmp/certbot/public_html/.well-known/acme-challenge
    cd /tmp/certbot/public_html
    printf "%s" R9CHAx3oXTzw0NhYX-YMM2cKgP4hErSy92CUO4HUSRE.YOUR_RANDOM_KEY > .well-known/acme-challenge/R9CHAx3oXTzw0NhYX-YMM2cKgP4hErSy92CUO4HUSRE
    # run only once per server:
    $(command -v python2 || command -v python2.7 || command -v python2.6) -c \
    "import BaseHTTPServer, SimpleHTTPServer; \
    s = BaseHTTPServer.HTTPServer(('', 80), SimpleHTTPServer.SimpleHTTPRequestHandler); \
    s.serve_forever()" 
    -------------------------------------------------------------------------------
    Press Enter to Continue


여기서 잠깐 멈추고.

SSL CA인 lets encrypt가 어떻게 도메인 주인을 확인하냐면,

http://bebetem.com/.well-known/acme-challenge/R9CHAx3oXTzw0NhYX-YMM2cKgP4hErSy92CUO4HUSRE

이 주소로 접근했을 때, 

R9CHAx3oXTzw0NhYX-YMM2cKgP4hErSy92CUO4HUSRE.YOUR_RANDOM_KEY

이 값을 받으면 해당 도메인 주인이 SSL certificate을 발급받는게 맞구나 하고 인식한다.


## well-known url 설정

routes.rb

    get '.well-known/acme-challenge/:id' => 'static#letsencrypt' 

static_controller.rb

    class StaticController < ApplicationController
    def letsencrypt
        render text: "#{params[:id]}.YOUR_RANDOM_KEY"
    end


heroku에 배포 후 SSL certificate 발급 절차를 다시 진행하면 아래와 같은 log를 확인할 수 있다

    Waiting for verification...
    Cleaning up challenges
    Generating key (2048 bits): /etc/letsencrypt/keys/0001_key-certbot.pem
    Creating CSR: /etc/letsencrypt/csr/0001_csr-certbot.pem

    IMPORTANT NOTES:
     - Congratulations! Your certificate and chain have been saved at
       /etc/letsencrypt/live/bebetem.com/fullchain.pem. Your cert will
       expire on 2017-05-14. To obtain a new or tweaked version of this
       certificate in the future, simply run certbot again. To
       non-interactively renew *all* of your certificates, run "certbot
       renew"
     - If you like Certbot, please consider supporting our work by:

       Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
       Donating to EFF:                    https://eff.org/donate-le



## Heroku에 certificate 등록


Heroku에서는 paid server를 사용하면 무료로 certificate을 등록할 수 있게 해주고 있다.
free server를 사용하는 경우는 잘 모름.

    heroku certs:add /etc/letsencrypt/live/bebetem.com/fullchain.pem /etc/letsencrypt/live/bebetem.com/privkey.pem


## DNS 변경

SSL certificate을 등록하고 나면 herokuapp의 end-point URL이 변경됩니다.
bebetem.herokuapp.com 에서 bebetem.com.herokudns.com으로 변경됨.

사용하는 DNS 서비스를 이용해서 CNAME을 your_app_name.herokuapp.com 에서 your_domain.herokudns.com 으로 변경해주어야 합니다.

dns 변경 확인

    > dig www.bebetem.com +short
    www.bebetem.com.herokudns.com




## Issue

### An unexpected error occurred: AttributeError: 'EntryPoint' object has no attribute 'resolve'

[github issue link][6]

brew를 최신버전으로 update 후에 certbot을 다시 설치

    brew update                 // brew update
    brew unlink certbot         // 설치된 certbot의 symbolic link 제거
    brew install certbot        // cerbot 재설치



---


[Heroku SSL][2]
[SSL에 대한 쉬운 설명 - kldp wiki][3]
[Let's encrypt CA 작동 설명][4]
[Rails codes reference - Daniel Morrison][5]

[1]: https://letsencrypt.org/
[2]: https://devcenter.heroku.com/articles/ssl#overview
[3]: https://wiki.kldp.org/HOWTO/html/SSL-Certificates-HOWTO/x70.html
[4]: https://letsencrypt.org/how-it-works/
[5]: https://collectiveidea.com/blog/archives/2016/01/12/lets-encrypt-with-a-rails-app-on-heroku
[6]: https://github.com/certbot/certbot/issues/3140#issuecomment-225274500
