---
title: 루비 버전 관리 - rbenv Mac 설치
layout: post
comments: true
category: [dev, ruby]
--- 

(rbenv github)[1]
(ruby-build)[2] : rbenv install 명령어를 사용가능하게 하는 plugin

### 설치

    git clone https://github.com/rbenv/rbenv.git ~/.rbenv
    git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile



### 간단한 사용법

    rbenv install -l        // 설치가능한 모든 ruby version list
    rbenv install 2.2.5     // 2.2.5 버전 ruby 설치
    rbenv uninstall 2.2.5   // 2.2.5 버전 ruby 삭제
    rbenv local 2.2.5       // 해당 폹더에서 2.2.5 버전 사용
    rbenv local             // 해당 폴더 루비 버전 확인




--

[1]: https://github.com/rbenv/rbenv
[2]:https://github.com/rbenv/ruby-build#readme


