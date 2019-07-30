---
layout: post
comments: true
title:  "의존성 주입과 그 선택 ( @Autowired, @Primary, @Qualifier )"
category: [backend, spring]
---
의존성 주입과 그 선택 ( Autowired, Primary, Qualifier )

@Autowired

스틱 자동차 대신 자동 변속 기어 자동차가 그러하듯이 자동은 언제나 편리하다. Autowired라는 Spring의 속성 또한 그렇다.

직관적으로 이해를 하자면 Auto+ wired. 자동으로 연결 되었다는 것이다. Spring을 써 보지 않았더라도, Naming이 아무렇게나 이루어지지 않았다는 걸 알았을 경우에는 자동 연결을 해줄 것이라는 것을 유추할 수 있게 된다.

개발 지식을 쌓으며 제일 중요한 것은 공식 문서를 보는 것이다.
재밌는 것은 google.co.kr에 @Autowired를 치면 첫 페이지에 Spring 공식 문서가 나오지 않는다. ( 문서를 보려고 굳이 뒤에 documentation을 추가 하였다.) 개인적으로 이 글을 읽는 분들이라도 항상 공식 doc을 보는 것을 추천한다. 물론 나도 블로그 글들을 자주 보고 정보를 빠르게 습득 하기 위해 이용하기는 하지만, doc이 있는 경우에는 항상 doc을 읽으려고 한다. 블로그의 정리 글은 빠르게 개념을 잡기 위함, 혹은 특정 상황에서 truble shooting을 할 때 좀 더 유용한 역할을 해 줄 수 있다.

돌아가서. 그럼 Autowired는 무엇인가, container autowires all beans matching the declared value type, 컨테이너는 선언되어 매칭되는 모든 Bean들을 autowired시켜준다.

optional한 element로는 required 를 boolean type으로 사용할 수 있다.

| Modifier and Type | Optional Element and Description                             |
| :---------------- | :----------------------------------------------------------- |
| `boolean`         | `required`Declares whether the annotated dependency is required. |

(출처 : 공식 doc)

하지만 Autowired를 사용하며 혼란이 있을 수 있으니, 이는 여러 candidate가 있을 때 이다.
이럴 때에는 @Bean에 @Primary를 사용함으로, 특정 bean에 높은 우선 순위를 부여할 수 있다. ( 하단과 같을 경우 first가 될 것이다.)

```java
@Configuration
public class MovieConfiguration {

    @Bean
    @Primary
    public MovieCatalog firstMovieCatalog() { ... }

    @Bean
    public MovieCatalog secondMovieCatalog() { ... }

    // ...
}
```



그러고보니, 잠깐, Autowired 공식 문서의 See also 탭에 [`AutowiredAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html) [`Qualifier`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html) 와 같은 것들이 등장한다.

전자에 대해서는 나중에 다루도록 하고 @Qualifier의 경우에는 어찌 보면 Primary와 유사하다. 후보 Bean에 대하여 qualifiy 한다는 것이데 둘은 차이가 있다.

@Primary의 경우에는 우선 순위를 올리는 것이고, @Qualifier 의 경우에는 명확하게 규정, 혹은 한정을 한다는 것이다. 




도움 받은 곳

백기선님의 인프런 강의, spring doc

https://docs.spring.io/spring/docs/4.3.12.RELEASE/spring-framework-reference/htmlsingle/#beans-autowired-annotation-primary

https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html

