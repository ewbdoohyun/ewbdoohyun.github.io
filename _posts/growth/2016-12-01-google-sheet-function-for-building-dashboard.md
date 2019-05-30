---
title: Google Sheet - 대시보드 만드는데 사용되는 function들
layout: post
comments: true
category: [growth, Measure]
--- 

![google-sheet-dashboard](/public/google-sheet.png)

전체 함수 리스트는 [google sheet function list][2] 에서 확인하세요.

Google Sheet를 이용해서 대시보드를 만들면서 사용한 함수들을 정리해봅니다.
Google Analytics Add-on을 사용해서 data를 가져왔습니다.(GA Add-on은 [google analytics add-on]({% post_url 2016-12-01-google-sheet-google-analytics-addon %})을 참고하세요.

<br>

## 공통 - Error Handling

자동으로 데이터를 가져오는 대시보드를 만들 때, 데이터가 없는 경우도 생각해야 합니다.

`에러 방생시 0을 반환`

    = iferror(에러가발생하는_연산, 0)    // = 0

<br>

## 하나의 column이 일치하는 row의 데이터 가져오기

`A column에서 "naver"를 찾아서, B column 값을 가져오기`

    = vlookup("naver", A:B, 2, false)

마지막 인자(false)는 exact match를 뜻합니다.

<br>

## 두개의 column이 일치하는 row의 데이터를 가져오기

`A column과 B column값이  "naver", "organic" 인 row의 C column 값 가져오기`

    =index( 
        filter(                 // filter()는 조건이 맞는 raw를 반환합니다.
            A:C,                // 조건을 확인할 영역
            A:A = "naver",      // 첫번째 조건
            B:B = "organic"     // 두번째 조건
        )
        , 3                     // 가져올 데이터의 column 순서 3 = c column
    )

가져올 데이터가 D column 이라면, filter의 range를 A:D로 바꾸고, index에 마지막 이자를 3에서 4로 바꿉니다.

*filter*는 여러개의 조건을 계속 설정할 수 있습니다.

*index*는 특정 row, column의 값을 가져오는 함수입니다. 
row가 여러개인 경우엔 index(range, , 1) 와 같이 row값을 생략하면 1번 column만으로 만들어진 cell array를 반환합니다.

<br>  

## 가변적인 데이터를 모두 가져오기

referral path처럼 데이터 숫자가 바뀔 수 있는 경우엔 비어있는 셀이 나올 때 까지 모든 데이터를 가져오는게 좋습니다.

`A column의 비어있지 않은 모든 데이터 가져오기`

    = filter(
        A:A, 
        isblank(A:A) = FALSE
        )

[credit - goole docs forum][3] 

<br>
<br>

## Formatting

<br>

### Conditional Formatting - custom fomula

custom fomula 사용시, 대상이 되는 셀의 값을 reference로 하려면, 선택한 영역의 제일 첫 cell을 넣어주면 됩니다.

`B30:B33 을 선택한 경우`

    = B30 > 0

`짝수 행 선택`

    = iseven(row())

`홀수 행 선택`

    = iseven(row())

`첫 문자가 '+' 인 경우`

    = left(B30, 1) = '+'

`끝 문자가 '.'인 경우`

    = right(B30, 1) = '.'

<br>  

`주의` 서로 다른 condition이 하나의 cell에서 충돌하면 conditional formatting이 제대로 동작하지 않습니다.

만약 짝수행 배경색을 넣는 것과, 양수/음수에 따라 글자색을 바꾸는 conditional formatting을 적용하려면 총 4가지 경우에 대해서 conditional formatting을 설정해 줘야 합니다.

    = and ( iseven(row()),  B30> 0)  //양수 & 짝수행
    = and ( iseven(row()),  B30< 0)  //음수 & 짝수행
    = and ( isodd(row()),   B30> 0)  //양수 & 홀수행
    = and ( isodd(row()),   B30< 0)  //음수 & 홀수행

<br>  

### Text

`text 끼리의 합치기`

    = "a" & "b"                 // "ab"
    = concatenate("a", "b")     // "ab"

<br>

#### text()

*text()*는 주어진 formatting을 적용한 text로 반환합니다.
`#` 은 자리수를 나타내며, 자리에 숫자가 없으면 빈칸이 됩니다.
반대로 `0`은 자리에 숫자가 없으면 0을 채워넣습니다.

`주로 사용하는 formatting list`

    "₩#,##0"
    "0.00%"
    "#,##0"

`원화 (값이 0일 때 ₩0 표시)`

    = text(12312, "₩#,##0")     // ₩12,312

`퍼센트 (값이 0일 때 0% 표시)`

    = text(0.11, "0.00%")      // 1.10%

숫자 (천단위 구분점, 값이 0일 때 0표시)

    = text(1123, "#,###")       // 1,123

<br>

#### 값이 양수일 때 '+' 기호 넣기

음수일 경우엔 기본적으로 '-' 기호가 표시되지만, 양수인 경우엔 '+' 기호가 표시되지 않습니다.

`A1이 B1보다 큰 경우에 '+' 기호 넣기`

    = if (
        A1 - B1 > 0, 
        concatenate("+", text(A1-B1, "#,##0")), 
        text(A1-B1, "#,##0")
        )


<br>

#### Indent

Google sheet에는 Indent 기능이 없지만, 아래와 같이 빈칸을 집어넣어 indenting 효과를 줄 수 있습니다.

    = REPT( CHAR( 160 ) , 15 ) & your_text  // 15를 바꾸면 원하는 indent depth를 조절할 수 있습니다.

<br>

## 기타

### 연산자
"<>" : 같지 않음

<br>

### 특정 에러를 타겟하는 방법

[Error.type][1] 은 에러 유형에 따라 숫자를 반환합니다.

Error Type List
1 for #NULL!
2 for #DIV/0!
3 for #VALUE!
4 for #REF!
5 for #NAME?
6 for #NUM!
7 for #N/A
8 for all other errors

`A1이 #NULL! 에러일 경우, 0으로 값을 바꾸기`

    = if(Error.type(A1) = 1, 0, A1)

[credit - forum][4]



---
reference
[10-techniques-for-building-dashboards-in-google-sheets][5]

[1]: https://support.google.com/docs/answer/3238305?hl=en
[2]: https://support.google.com/docs/table/25273?hl=en
[3]: https://productforums.google.com/forum/#!topic/docs/6B-wbF1Om1g
[4]: https://productforums.google.com/forum/#!topic/docs/q_As66-Q3P4
[5]: http://www.benlcollins.com/spreadsheets/10-techniques-for-building-dashboards-in-google-sheets/
