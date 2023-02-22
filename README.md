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
    
    
  

<br></br>
<br></br>


**7. table 태그, th 태그, tr 태그, td 태그**
```
 <table> : 표를 만드는 태그
 <th> : table head의 약자로, 표의 제목을 쓰는 역할
 <tr> : table row의 약자로, 표의 가로줄을 만드는 역할
 <td> : table data의 약자로, 셀을 만드는 역할
```

<br></br>

<예시>
```html
<table class="table table-striped">
    <thead>
    <tr>
        <th> 이름 </th>
        <th> 도시 </th>
        <th> 주소 </th>
        <th> 우편번호 </th>
    </tr>
    </thead>
    <tbody>
    <tr th:each="member : ${members}">
        <td th:text="${member.name}"></td>
        <td th:text="${member.address?.city}"></td>
        <td th:text="${member.address?.street}"></td>
        <td th:text="${member.address?.zipcode}"></td>
    </tr>
    </tbody>
</table>

```
  
 
 
 
<br></br>
<br></br>

**8. @RequestParam vs @PathVariable**


주로 사용하는 형태의 파라미터를 전달하는 경우
```
**Case 1.** http://xxxxx? index=1 & page=2
**Case 2.** http://xxxxx/ index/1
```
**Case1** 의 경우, 파라미터의 값과 이름을 함께 전달하는 방식으로, 게시판 등에서 페이지 및 검색 정보를 함께 전달하는 방식을 사용할때 많이 사용한다.

**---> @RequestParam 사용**
<br></br>

**Case2** 의 경우, REST API에서 값을 호출할 때 많이 사용한다.

**---> @PathVariable 사용**


<br></br>

<예시>
```java

// @RequestParam 사용

@GetMapping("/home")
public String myHome( @RequestParam("name") String name, @RequestParam ("phone") phone ){
    viewName( name, phone);
}


// @RequestParam의 파라미터가 많아지는 경우, Map 사용

@GetMapping("/home")
public String myHome( @RequestParam HashMap <String, String> paramMap ){
    String data = param.get("name")
}



// @PathVariable

@GetMapping("/home")
public void myHome( @PathVariable("idx") int id ){
    return viewName(id);
}


// 복합 사용
@GetMapping("test")
public List<Test> testMethod( @PathVariable("idx") int id, @RequestParam(value="date" ,required="false) Date userDate) {

}

```


 
<br></br>
<br></br>


**9. [Thymeleaf] th:field**


```
th:field ="*{필드 명}"
```

설정을 통해 HTML 필드와 폼 객체에 포함된 필드를 연결할 수 있고, HTML 필드 값이 폼 객체의 해당 필드로 설정된다.


        
 
<br></br>
<br></br>


**10. @RequestBody vs @ResponseBody**
<br></br>


@RequestBody와 @ResponseBody는 스프링에서 비동기 처리를 할 때 사용하는 어노테이션이다.

**클라이언트**에서 **서버**로 통신하는 메시지를 **요청 메시지**, **서버**에서 **클라이언트**로 통신하는 메시지를 **응답 메시지**라 한다.

웹에서 화면 전환(새로고침) 없이 이루어지는 동작들은 대부분 **비동기 통신**으로 이루어진다.

비동기 통신을 위해서는 클라이언트에서 서버에 요청 메시지를 보낼 때, 본문(요청 본문, requestBody)에 데이터를 담아서 보내야하고,

서버에서 클라이언트에 응답 메시지를 보낼때, 본문(응답 본문, responseBody)에 데이터를 담아서 보내야 한다.

```
@RequestBody : 요청 본문에 담긴 값을 자바 객체로 변환시켜 객체에 저장한다.

@ResponseBody : 자바 객체를 HTTP 응답 본문의 객체로 변환시켜 클라이언트로 전송한다.
```

* @RestController = @Controller + @ResponseBody


```java
@ResponseBody
@RequestMapping(value = "/ajaxTest.do")
public UserVO ajaxTest() throws Exception {

  UserVO userVO = new UserVO();
  userVO.setId("테스트");

  return userVO;
}
```


```java
@PutMapping("/api/v2/members/{id}")
public UpdateMemberResponse updateMemberV2( @PathVariable("id") Long id, @RequestBody @Valid UpdateMemberRequest request) {

  memberService.update(id, request.getName());
  Member findMember = memberService.findOne(id);
  return new UpdateMemberResponse(findMember.getId(), findMember.getName());
  
}

```


        
 
<br></br>
<br></br>


**11. @Valid**

파라미터로 @RequestBody 앞에 @Valid를 작성하면, @RequestBody로 들어오는 객체에 대한 검증을 수행한다. 이 검증의 세부사항은 객체 안에 정의해놓아야 한다.

```java
public CreateMemberResponse saveMember(@RequestBody @Valid UserDto userDto){
    Long id = memberService.join(userDto);
    return new CreateMemberResponse(id);
}
```

```java
public class UserDto {

    @NotNull
    private String name;
    
    @Email
    private String email;
}
```
여기서는 @NotNull, @Email 형식을 갖추는지 @Valid가 검증한다.


         
<br></br>
<br></br>


**12. @Data**

lombok의 어노테이션 @Data는 @Getter, @Setter, @RequiredArgsConstructor, @EqualsAndHashCode를 합쳐놓은 것과 같다. 


<br></br>
<br></br>

**13. @NoArgsConstructor, @AllArgsConstructor, @RequiredArgsConstructor**


@NoArgsConstructor : 파라미터가 없는 기본 생성자를 생성

@AllArgsConstructor : 모든 필드 값을 파라미터로 받는 생성자를 만듦

@RequiredArgsConstructor : final이나 @NonNull인 필드 값만 파라미터로 받는 생성자 만듦


```java
@NoArgsConstructor
@RequiredArgsConstructor
@AllArgsConstructor
public class User {

  private Long id;
  
  @NonNull
  private String name;
  
  @NonNull
  private String pw;
  
  private int age;
  
}
```

```java
User user1 = new User(); // @NoArgsConstructor
User user2 = new User("user2", "1234"); // @RequiredArgsConstructor
User user3 = new User(1L, "user3", "1234", null); // @AllArgsConstructor
```



<br></br>
<br></br>

**14. Stream.collect() 사용**

Stream.collect()의 기능

1. Stream의 아이템들을 List 또는 Set 자료형으로 변환
2. Stream의 아이템들을 joining
3. Stream의 아이템들을 Sorting하여 가장 큰 객체 리턴
4. Stream의 아이템들의 평균값 리턴


```java
Stream<String> fruits = Stream.of("banana", "apple", "mango", "kiwi", "peach", "cherry", "lemon");
Set<String> fruitSet = fruits.collect(Collectors.toSet());
for (String s : fruitSet) {
    System.out.println(s);
}
```


<br></br>
<br></br>


**15. @Bean vs @Component**

```
@Bean : 개발자가 컨트롤이 불가능한 외부 라이브러리들을 Bean으로 등록하고 싶은 경우에 사용한다.

@Component : 개발자가 직접 컨트롤이 가능한 Class들의 경우에는 @Component를 사용한다.
```

@Bean과 @Component는 각자 선언할 수 있는 타입이 정해져있어 해당 용도외에는 컴파일 에러를 발생시킨다.



<br></br>
<br></br>


**16. @NoArgsConstructor(access=AccessLevel.PROETECTED)**


protected : 다른 패키지에 소속된 클래스는 접근할 수 없는 클래스


무분별한 객체 생성을 방지할 수 있다.  -> 기본 생성자 막고 싶은데. JPA 스펙상 PROTECTED로 열어두어야 한다.



<br></br>
<br></br>


**17. @ToString**

@ToString을 클래스에 사용하면 toString() 메소드를 자동으로 생성해준다. exclude 속성을 사용하면 특정 필드만 제외시킬 수 있다. @ToString(exclude="name")

@ToString은 가급적 내부 필드만 

```java
@ToString(of={"id","name"})
public class Team{
  
  private Long id;
  private String name;

}
```

```java

System.out.println(Team);

```
Team(id=1,name="youngseon")

클래스명(필드1명=필드1값,필드2명=필드2값,...) 식으로 출력







