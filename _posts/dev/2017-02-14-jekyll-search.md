---
title: Jekyll에 Search 기능 넣기 Lunrjs
layout: post
comments: true
category: [dev, jekyll]
--- 



house201 블로그는 jekyll을 이용해서 만들었다.
다 좋은데 static page 기반이라 검색이 안된다는게 조금은 불만.

[Jekyll.tips][1]에 올라온 screen cast를 보고 검색기능을 넣어보기로 했다.



## Jekyll tips codes

search.html

    {% raw %}
    ---
    layout: search
    ---
    
    <div id="search-results"></div>
    
    <script>
      window.store = {
        {% for post in site.posts %}
          "{{ post.url | slugify }}": {
            "title": "{{ post.title | xml_escape }}",
            "category": "{{ post.categories | xml_escape }}",
            "published_at" : "{{post.date | date: '%b %d, %y' }}",
            "url": "{{ post.url | xml_escape }}"
          }
          {% unless forloop.last %},{% endunless %}
        {% endfor %}
      };
    </script>
    <script src="/js/lunr.min.js"></script>
    <script src="/js/search.js"></script>
    {% endraw %}


`window.store` 에 모든 검색 대상을 저장하는 방식이다. post양이 엄청 많다면 browser가 느려질 수 있겠지만. 제목과 카테고리 정도라면 큰 문제는 안 될 것 같다.



_layouts/search.html

    {% raw %}
    ---
    layout: default
    ---
    
    {{content}}

    {% endraw %}


js/search.js


    (function() {
      function displaySearchResults(results, store) {
        var searchResults = document.getElementById('search-results');
    
        if (results.length) { // Are there any results?
          var appendString = '';
    
          for (var i = 0; i < results.length; i++) {  // Iterate over the results
            var item = store[results[i].ref];
            appendString += '<div class="search__result"><a href="' + item.url + '"><h5 class="post-title">' + item.title + '</h5></a>';
            appendString += '<div class="post-description"><small class="post-date">' + item.published_at + '</small></div>';
            appendString += '</div>'
          }
    
          searchResults.innerHTML = appendString;
        } else {
          searchResults.innerHTML = '<li>No results found</li>';
        }
      }
    
      function getQueryVariable(variable) {
        var query = window.location.search.substring(1);
        var vars = query.split('&');
    
        for (var i = 0; i < vars.length; i++) {
          var pair = vars[i].split('=');
    
          if (pair[0] === variable) {
            return decodeURIComponent(pair[1].replace(/\+/g, '%20'));
          }
        }
      }
    
      var searchTerm = getQueryVariable('query');
    
      if (searchTerm) {
        document.getElementById('search-box').setAttribute("value", searchTerm);
    
        // Initalize lunr with the fields it will be searching on. I've given title
        // a boost of 10 to indicate matches on this field are more important.
        var idx = lunr(function () {
          this.field('id');
          this.field('title', { boost: 10 });
          this.field('category');
        });
    
        for (var key in window.store) { // Add the data to lunr
          idx.add({
            'id': key,
            'title': window.store[key].title,
            'category': window.store[key].category,
          });
    
          var results = idx.search(searchTerm); // Get lunr to perform a search
          displaySearchResults(results, window.store); // We'll write this in the next section
        }
      }
    })();


`displaySearchResults` 이 부분은 검색결과를 어떻게 화면에다가 붙여줄 건지에 대한 부분

`getQueryVariable` 이 부분은 query로 들어온 값을 검색어 배열로 바궈주는 부분

나머지 부분이 lurn에 모든 포스트 정보를 집어넣고, 검색어에 따라 일치하는 정보만 가져오는 부분이다.



## js/lunr.min.js

공식 릴리즈 버전은 한글 검색이 지원이 안되더라. 다 만들고나서 멘붕하고 있었는데, 대부분 이럴 때 korean support보다 chinese support로 검색하면 원하는 답을 찾는 경우가 많았다.

정말 능력이 좋아서 korean support 버전을 척척 만들어내면 좋겠지만, 나는 어디까지나 필요한 것을 잘 조합해서 쓰고 싶은 정도니깐.

아래 chinese support 버전에서 lurn.min.js를 다운받아 사용해 보니 한글 검색도 잘 된다!

[lurn chinese support version][2]


<br>

언제나 할 때는 헤메다가, 다 하고 블로그로 쓰니 정말 쉬워보인다.

--

references

- [jekyll.tips][1]
- [lunr chinese support][2]

[1]: http://jekyll.tips/jekyll-casts/jekyll-search-using-lunr-js/
[2]: https://github.com/codepiano/lunr.js
