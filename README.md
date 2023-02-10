# Review-note

**1. @RequestMapping과 @GetMapping 의 차이**
 
   : @RequestMapping은 클래스와 메서드 둘다 매핑 가능하지만, @GetMapping은 메서드만 매핑가능하다.   
   
  
<br></br>  
<br></br>








**2. 타임 리프의 meta 태그**
  
   - http-equiv="항목명"
   
      : 웹브라우저가 서버에 명령을 내리는 속성으로, name 속성을 대신하여 사용할 수 있으며, HTML문서가 응답헤더와 함께 웹브라우저에 전송되었을 때에만 의미를 갖는다.
      
   
   - content="정보값" 
      : meta 정보의 내용을 지정한다.
     
     
   - name="정보이름" 
      : 몇개의 메타 정보 이름을 지정할 수 있으며, 지정하지 않으면 http-equiv와 같은 기능을 한다.
     
    
    ex) <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" /> 컨텐츠 타입정보 표시
    
    ex) <meta name="Date" content="2021-07-20T07:45:37+09:00" /> 제작 날짜 표시
    
<br></br>
<br></br>










**3. 필드 주입 vs 생성자 주입**

   
   
   - 필드 주입

```java
public class Service {

    @Autowired
    private MemberRepository memberRepository;
    
}
```

   - 생성자 주입 
   
```java
public class Service {

    @Autowired  // 생성자가 하나면 생략가능
    private final MemberRepository memberRepository;
    
    public Service(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    
}
```
생성자 주입을 통해, 변경 불가능한 안전한 객체 생성이 가능하고,  'final' 키워드를 추가하면 컴파일 시점에 memberRepository를 설정하지 않은 오류를 체크할 수 있다.

<br></br>
<br></br>








**4. @RequiredArgsConstructor 의 기능**

: 생성자 주입의 코드처럼 생성자를 만들기 번거로울때, 이를 보완하기 위해 롬복의 @RequiredArgsConstructor를 사용하면 된다.

- @RequiredArgsConstructor : final이 붙거나 @NotNull이 붙은 필드의 생성자를 자동 생성해주는 롬복의 어노테이션이다.

<기존 생성자 주입>

```java
@Service
public class Service {

    @Autowired  // 생성자가 하나면 생략가능
    private final MemberRepository memberRepository;
    
    public Service(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    
}
```

<@RequiredArgsConstructor사용 후>
```java
@Service
@RequiredArgsConstructor
public class Service {

    private final MemberRepository memberRepository;
    
}
```

<br></br>
<br></br>






**5. @Test (expected = Excpetion 클래스명 )**
```java

@Test(expected = IllegalStateException.class)
public void 중복확인() throws Exception {
    
    // Given
    Member member1 = new Member();
    member1.setName("youngseon");
    
    Member member2 = new Member();
    member2.setName("youngseon");
    
    //When
    memberService.join(member1);
    memberService.join(member2);
    
    /**
    여기서 IllegalStateException이 터져 테스트는 pass된 채로 테스트가 종료된다.
    */
    
    //Then 
    
    fail("예외가 발생해야 한다");
    /**
    예외가 터지지 않고 fail까지 넘어오면, fail이 "예외가 발생해야 한다" 라는 문구를 보여주며 테스트를 fail 시킨다.
    */
}
```


<br></br>
<br></br>



**6. [JPA] Entity 상태와 Cascade 옵션**

- Entity의 상태
```
1. Transient : 객체를 생성하고, 값을 주어도 JPA나 Hibernate가 그 객체에 관해 아무것도 모르는 상태이다. 즉, 데이터베이스와 매핑된 것이 아무것도 없다.
2. Persistent : EntityManager를 통해 영속성 컨텍스트에 저장을 하고나서, 영속성 컨텍스트가 관리하는 상태가 된다. 
3. Detached : 영속성 컨텍스트에 저장되었다가 분리된 상태로, 영속성 컨텍스트가 더이상 관리하지 않는 상태이다.
4. Removed : Entity를 영속성 컨텍스트와 DB에서 삭제한 상태이다.
```
<br></br>
- Cascade 옵션

    : Parent와 Child 관계에만 적용할 수 있는 옵션으로, CascadeType.java에는 ALL, PERSIST, MERGE, REMOVE, REFRESH, DETACH 속성이 있다.

- Cascade 사용 예시
```java
OneToMany (mappedBy = "order", cascade = CascadeType.ALL)
```
-> Parent Entity의 영속성을 Child Entity도 전이 받는다. 즉, 부모 엔티티를 통해 자식 엔티티의 생명주기가 관리된다.
    
    
  






