# File Upload

## 개요
 웹 어플리케이션에서 기본적인 파일 업로드와 다운로드를 하는 시나리오를 알아보자

## Multipart
웹 클라이언트가 요청을 보낼 때 HTTP 프롵토콜의 바디 부분에 데이터를 여러 부분으로 나눠서 보내는 것을 말한다.<br>
보통 파일을 전송할 때 사용한다.



폼을 처리할 때 인풋 타입을 파일로 주고 사용
일반적인 form 데이터를 전송 시에 HTTP Header에는 기본적으로, 'application/x-www-form-urlencoded' 라는 정보로 노출 된다.
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7rKYKhIWaTrjvGi1