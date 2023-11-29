# File Upload

## 개요
 웹 어플리케이션에서 기본적인 파일 업로드와 다운로드를 하는 시나리오를 알아보자

## Multipart
웹 클라이언트가 요청을 보낼 때 HTTP 프롵토콜의 바디 부분에 데이터를 여러 부분으로 나눠서 보내는 것을 말한다.<br>
보통 파일을 전송할 때 사용한다.
HttpServletRequest는 Multipart데이터를 처리하는 메소드를 제공하지 않는다. 이런 이유로 별도의 라이브러리를 사용해야하는데 그중 하가 아파치 재단의 commons-fileupload이다.
디스패쳐 서블릿은 준비과정에서 멀티파트 폼 데이터가 요청으로 올 경우 멀티파트 리졸버를 사용한다.
파일 업로드시(프론트) form태그에 endtype이 설정되어있어야한다.(멀티파트로) 그리고 인풋의 타입을 파일로 설정해줘야한다. 이름이 같다면 리스트 형식으로 전달된다.
포스트 매소드 사용. 파라미터로 전달되며 멀티파트파일의 메소드를 이용해서 파일이름, 파일 크기 등을 구하고 인풋스트림을 얻어 파일을 서버에 저장한다.
<input type="file" name="reviewImg" id="reviewImageFileOpenInput" accept="image/*">

폼을 처리할 때 인풋 타입을 파일로 주고 사용
일반적인 form 데이터를 전송 시에 HTTP Header에는 기본적으로, 'application/x-www-form-urlencoded' 라는 정보로 노출 된다.
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7rKYKhIWaTrjvGi1