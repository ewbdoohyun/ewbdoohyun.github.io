---
layout: post
comments: true
title:  "JPA Join 한번에 정복하기 ( OneToOne, OneToMany, ManyToOne ) , 발생할 수 있는 문제 ( N+1 , Json exception)"
category: [backend, spring]
---

JPA를 사용할 경우 

다음은 Join 관계를 정의하기 위한 Table이다. MemberInfo의 경우 단순히 OneToOne을 설명하기 위해 정의하였다. 

```java
@Table
@Entity
 class Memeber{  /// one
    @Id
    @Column
    private long   componentId;
    private MemberInfo memberInfo;
    private List<Post> posts;
 }

@Table
@Entity
class MemberInfo{   /// member 1: memberInfo 1 
    @Id
    private long componentId;
    @Column
    private long memberId;
    private Member member;

}

@Table
@Entity
 class Post{   //member1 : memberInfo 1
     @Id
     private long componentId;
     @Column
     private long memberId;
     private Member member;

 }
```

관계를 간단하게 정리하자면 Memeber와 MemberInfo는 1:1 관계이고, Member와 Posts는 1:N 관계로 만드려는 의도가 있다..
하지만 지금의 상태는 서로 클래스 간 Mapping이 되어 있지 않은 상태이다. 즉 내가 post들을 가져오려면

 1. Member를 가져온다.
 2. Member 정보를 기반으로 MemberInfo를 가져와 채워 넣는다.
 3. Member 정보를 기반으로 Post들을 가져와 채워 넣는다.

의 세 단계로 이루어진 작업을 하여야 한다. 

DB Query를 사용할 경우에는 이것을 Join (Inner, outer 등등) 으로 해결 하고 있고,
JPA 또한 이를 Join으로 해결 하고 있는데. 방법은 다음과 같다.

One To One의 예
```java
@Table
@Entity
 class Memeber{  /// one
    @Id
    @Column
    private long   componentId;
    @OneToOne
    @JoinColumn(name="memberId")
    private MemberInfo memberInfo;
 }

@Table
@Entity
class MemberInfo{   /// member 1: memberInfo 1 
    @Id
    private long componentId;
    @Column
    private long memberId;
    @OneToOne
    @JoinColumn(name = "componentId")
    private Member member;

}
```
    위와같이 부모는 자식의 column, 자식은 부모의 column을 명시하면 양방향으로 mapping을 수행 할 수 있다.
    그런 후에 

```java
// repository는 생략함
class MemberService{
    @Autowired
    MemberRepository memberRepository;

    public List<Member> findByMemberIdsIn(List<Long> memberIds){
        List<Member> = memberRepository.findByMemberIdsIn(memberIds);
    }
}
```
  와 같이 호출을 하면 가져올 수 있다.
  그런데,  이와 같이 class를 작성하였을 때 문제가 발생할 수 있는 문제는 총 세가지가 있다.
 
  1. 내가 원하지 않을 경우에도 강제로 MemberInfo를 가져와야 함.
  2. DB에 대한 Call Long를 확인해 보면 Member를 한 번 호출하고, 각 Member 별로 MemberInfo를 호출한다. ( N+1번의 호출 횟수를 가지게 되는 문제 )
  3. 해당 객체를 서비스 밖으로 빼 내 api의 return으로 사용할 경우, 재귀에 의한 Exception이 발생 - Stackoverflow (Member는 memberInfo를, memberInfo는 member를 호출하게 되는 ) 

한 가지씩 해결 방안에 대하여 언급하자면

 1. @OneToMany, @ManyToOne, @OneToOne에 다음과 같은 옵션을 추가한다.

```java
    @OneToOne(fetch = FetchType.LAZY)
```
 
말 그대로 LAZY한 Fetching을 수행하겠다는 것인데, 이러면 필요할 떄만 연관된 테이블을 가져올 수 있게 된다. ( 해당 객체에 접근 할 떄만 가져옴)
그러면, 필요할 때라는 것은 어떤 상황을 의미하는가.
 1. 객체에 대한 실제 접근하여 값을 가져올 때
 2. repository에 relation을 강제로 맺을 경우

다음과 같은 두 가지 케이스에 대하여 가능한다.
 1의 경우 객체의 내부 값을 접근하기 때문에 별도 언급이 없이 이해가 되나, 2번의 경우는 repository에 다음과 같은 옵션을 추가하여야 한다.
 또한 2번 ( N+1 문제 ) 의 해결 방법도 다음과 같다.
```java
  @RepositoryRestResource
  public interface MemberRepository extends JPARepository<Member, Long>{
      @Qyery("SELECT DISTINCT m FROM Member m JOIN FETCH m.memberInfo mI JOIN FETCH m.posts p")
      List<Member> findByMemberIdsIn(@Param("memberIds") List<Long> memberIds);
  }
```

다음과 같이 Query로 명시적인 mapping 관계를 맺게 되면, 각 member 별로 하위의 data를 가져오는 사항에 대하여 해결 할 수도 있고, Lazy를 설정해 놓았으나, 한번에 하위 까지 같이 가져와야 하는 repository의 method를 만들경우 다음과 같이 운용할 수 있다.

그럼 이제 딱 하나 세 번쨰의 문제가 남았다.

해당 객체를 통채로 return 받게 되면, 데이터를 받는 서비스 바깥에서 stachoverflow가 발생한다.
문제는 json의 연관 관계가 맺어져 있지 않아, 서로가 서로를 참조하여 데이터를 무한정 양산해 내기 때문이다.
이를 해결하기 위해 json의 참조 관계를 만들어 주어야 한다.

```java
@Table
@Entity
 class Memeber{  /// one
    @Id
    @Column
    private long   componentId;
    @OneToOne
    @JoinColumn(name="memberId")
    @JsonBackReference  // parent
    private MemberInfo memberInfo;
 }

@Table
@Entity
class MemberInfo{   /// member 1: memberInfo 1 
    @Id
    private long componentId;
    @Column
    private long memberId;
    @OneToOne
    @JoinColumn(name = "componentId")
    @JsonManagedReference   // son
    private Member member;

}
```

OneToMany와 ManyToOne의 경우 위의 내용으로 설명이 같이 되었을 것이라 생각한다.
다만 OneToMany는 한 객체가 여러 객체를 참조할 경우 (ex member가 소유하는 posts) 
ManyToOne은 여러객체가 한 객체를 참조할 경우 ( 여러 post가 한 member를 참조하기 떄문 )
를사용하면 된다.

끝!