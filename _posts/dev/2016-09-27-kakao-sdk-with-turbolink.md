---
title: Turbolinks - Kakao sdk
layout: post
comments: true
category: [dev, rails]
--- 

Codes Only.


kakao_story.js

    $(document).on("turbolinks:load", function (){
        if ( $('#kakaostory-share-button').length > 0 ) {
            Kakao.Story.createShareButton({
          container: '#kakaostory-share-button'
        });
      }
    });

application.html.erb

    <head>
        <%= render 'layouts/kakao-sdk' %>
        ...
    </head>
    <body>
        <%= render 'layouts/kakao-sdk-init' %>
        ...

_kakao-sdk-init.html.erb

    <script type='text/javascript'>
      if ( typeof Kakao != "object") {
        Kakao.init(ENV["KAKAO-JS-KEY"]);
      }
    </script>

_kakao-sdk.html.erb

    <script src="//developers.kakao.com/sdk/js/kakao.min.js"></script>

