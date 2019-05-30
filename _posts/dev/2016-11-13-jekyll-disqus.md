---
title: Jekyll에 disqus 추가하기
layout: post
comments: true
category: [dev, jekyll]
--- 


모든 글에 대해서 disqus를 적용하려면 `post.html`에 다음을 추가하면 된다.

    <div id="disqus_thread"></div>
    <script>
        /**
         *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
         *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
         */
        /*
        var disqus_config = function () {
            this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
            this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
        };
        */
        (function() {  // DON'T EDIT BELOW THIS LINE
            var d = document, s = d.createElement('script');
            
            s.src = '//growthhackingmeeting.disqus.com/embed.js';
            
            s.setAttribute('data-timestamp', +new Date());
            (d.head || d.body).appendChild(s);
        })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>


글마다 disqus적용옵션을 설정하려면 `comments: true` 옵션을 글 머리에 넣고, `post.html`에는 `page.comments`가 있을 때만 위 코드를 삽입하면 된다.


    //some post
    ---
    title: ###
    layout: post
comments: true
    comments: true
    ...
    ---

    //post.html
    ...
    {% if page.comments %}
        여기에 disqus script 삽입
    {% endif %}


---

[https://help.disqus.com/customer/portal/articles/472138-jekyll-installation-instructions](https://help.disqus.com/customer/portal/articles/472138-jekyll-installation-instructions)
