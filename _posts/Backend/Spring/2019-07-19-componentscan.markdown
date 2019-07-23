---
layout: post
comments: true
title:  "ComponentScan에 대하여"
category: [Backend, spring]
---

들어가기 전에 : @SpringbootApplication anotation 안에도 있다.

```java
package org.springframework.boot.autoconfigure;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    
}
```

@ComponentScan 의 경우 
@Component annotation이 포함되어 있는 것들을 Scan할 수 있다.
대표적인 예로 @Repository, @Service, @Controller 등이 있다.
(관련한 정보는 spring docs 참조)

```java
@Configuration
@ComponentScan(basePackageClasses = DemoApplication.class) //여기가 위치한 곳 부터
public class ApplicationConfig {
	
    @Bean
    public BookRepository bookRepository() {
        return new BookRepository();
    }
}

```

혹은 다음과 같이 직업 Config를 설정하여 사용할 수 있다.

작성자는 DemoApplication.class 라고 적어 놓았는데, 이는 DemoApplication이 위치한 Depth부터 탐색 하겠다는  뜻이다.

(추후 업데이트 가능성 있음. 개인 사정상 블로그를 잠깐 쉬게 되었어서 주기화를 위해 짧게나마 기록 해 놓습니다.)



출처 : https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/stereotype/Component.html
인프런 스프링프레임워크 핵심기술