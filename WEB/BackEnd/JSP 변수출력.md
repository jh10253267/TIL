# JSP 변수 출력

## 개요

웹 백엔드를 공부해봤다면 서버에서 처리한 데이터를 뷰에 표현하는 것이 상당히 중요한 것임을 알 수 있다. 그래야 클라이언트에게 화면을 보여주고 개발한 서비스를 제공할 수 있다.

이번엔 Servlet과 JSP를 이용한 개발에서 화면에 표현하는 부분을 알아보자.

그러기 위한 것이 일단 JSP의 표현식이다.

서블릿에서 처리한 데이터를 request의 setAttribute()를 이용해 JSP에 전달한다.

이를 자바 화면에 출력한다.

그러나 만약 값이 null이라면 화면에 null이 바로 출력되어 이를 체크하고 null이라면 아무 것도 없는 문자열을 출력해야한다.

EL을 사용하면 JSP의 기본 문뱝들을 보완할 수 있다.

## 시용법

예를 들어
```jsp
pageContext.setAttribute("p1", "page scope value");
request.setAttribute("r1", "request scope value");
session.setAttribute("s1", "session scope value");
application.setAttribute("a1", "application scope value");
.
.
.
<body>
pageContext.getAttribute("p1") : <%=pageContext.getAttribute("p1")%> <br>
pageContext.getAttribute("p1") : ${pageScope.p1}
</body> 
```
원래의 JSP에선 위와 같이 사용하는데 EL을 사용한다면 밑과 같이 사용할 수 있다.

결과는 같다.

위에서 변수들을 셋팅해준 뒤 출력을 할 때 만약 각각 변수의 이름이 다르다면 앞의 pageScope와 같은 부분을 생략하여 `${p1}`과 같이 사용할 수 있다.

만약 변수가 겹친다면 작은 스코프부터 찾기 시작한다. 그러나 명확하게 사용하려면 앞의 객체를 명시해두는 것이 좋다.

## JSTL

 JSTL이란 Java Standard Tag Library의 약자로 JSP 페이지에서 조건문 처리, 반복문 처리등을 html 태그 형태로 작성할 수 있게 한다.

 이는 JSP의 스크립트렛 코드와 html의 태그가 혼재되어있어 유지보수가 어렵다는 문제가 있다. 이를 해결하기 위해 등장한 것이 바로 JSTL이다.

 JSTL을 사용하기 위해선 파일의 상단에 태그 설정을 해주어야한다.
 `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
`
이와 같이 <%@ %>로 작성되는 코드를 지시자라고 한다.

### <c:forEach>

반복문의 처리는 <c:forEach>를 사용한다.

```jsp
<ul>
  <c:forEach var = "dto" items="${list}">
    <li>${dto}</li>
  </c:forEach>
</ul>
```

여기서 var은 el에서 사용될 변수명을 말하고 items는 서버에서 넘어온 데이터를 말한다.

begin과 end속성도 있는데 이는 items에서 값을 읽어올 인덱스의 시작값을 말하고 end는 items에서 값을 읽어올 인덱스의 끝값을 말한다.

### <c:if> <c:choose>

JSTL에서의 제어문은 `<c:if>`와 `<c:choose>`를 사용한다.

```jsp
<c:if test="${list.size() % 2 == 0}">
  짝수
</c:if>
```
위의 코드는 리스트의 사이즈가 짝수라면 태그 내부의 내용을 처리한다.

`<c:choose>`는 조건에 따라 분기하며 내용을 처리한다.

```jsp
<c:choose>
    <c:when test="${score >=90 }">
    A학점입니다.
    </c:when>
    <c:when test="${score >=80 }">
    B학점입니다.
    </c:when>
    <c:when test="${score >=70 }">
    C학점입니다.
    </c:when>
    <c:when test="${score >=60 }">
    D학점입니다.
    </c:when>
    <c:otherwise>
    F학점입니다.
    </c:otherwise>            
</c:choose>
```






