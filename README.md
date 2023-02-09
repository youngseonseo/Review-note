# Review-note

**1. @RequestMapping과 @GetMapping 의 차이**
 
   : @RequestMapping은 클래스와 메서드 둘다 매핑 가능하지만, @GetMapping은 메서드만 매핑가능하다.   
   
  
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

**3.**
