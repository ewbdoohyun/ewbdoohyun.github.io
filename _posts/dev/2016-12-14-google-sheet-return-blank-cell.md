---
title: Google Sheet - 함수에서 blank cell 반환하기
layout: post
comments: true
category: [dev, google sheet]              
--- 

꼼수

    = iferror(1/0)


Error가 발생시 Blank cell return

    = iferror(operation_you_want, iferror(1/0))




---
credit : [http://googledocsforlife.blogspot.kr/2011/08/iferror10-in-spreadsheets.html][1]


[1]:http://googledocsforlife.blogspot.kr/2011/08/iferror10-in-spreadsheets.html
