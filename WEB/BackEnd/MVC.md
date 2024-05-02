# MVC

## 개요

지금 사용하고 있는 것은 갑작스럽게 만들어진 것이 아니라 기존의 문제를 해결하려고 만들어진 경우가 대부분이다.
MVC 패턴도 그렇다.

## MVC

MVC는 Model View Controller의 약자이다.

View는 사용자가 보는 화면을 말하고 Controller는 사용자의 입력을 받아 처리하는 곳을 말하고 Model이란 View와 Controller를 제외한 것을 말한다.

JSP를 사용한다고 해보자. JSP는 서블릿이기 때문에 자바 로직을 사용할 수 있고 HTML파일이기도 해서 View의 역할을 한다.

그러나 만약 JSP에 로직을 작성하였고 굉장히 많은 JSP파일들과 서블릿과 자바 클래스들이 있다고 가정해보면 
자바 코드에는 없는 동작이 JSP에는 존재해서 문제를 찾기 힘들어지는 상황이 발생할 수 있다. 또한 내부의 자바 코드를 재사용하는 문제도 생길 수 있다.

이는 역할이 구분되지 않고 혼용되어 사용되기 때문이다.

이를 해결하려면 JSP는 단순히 뷰의 역할만 하고 로직은 자바에서 처리하도록 하는 방법을 생각해볼 수 있다.

그리고 컨트롤러의 역할을 하는 서블릿 내부에 로직들이 모듈화되지 않고 있으면 중복되는 코드가 생길 수 있다.

따라서 데이터는 컨트롤러에서 처리하고 결과는 뷰에서 처리하고 그 외의 로직을 묶어서 그 중간 과정을 모델로 분리하여 운영하는 패턴이 바로 MVC이다.