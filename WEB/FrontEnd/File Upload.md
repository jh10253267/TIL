# File Upload

우리는 프론트 단에서 정보를 보낼 때 form태그와 input을 이용한다. form태그의 속성에 경로와 메소드를 명시해주고 submit을 하면 된다. 근데 우리는 글이나 숫자를 양식에 맞게 작성한 뒤 제출하기도 하지만 리뷰 등록과 같이 이미지 파일을 업로드하거나 원서 접수를 할 때 사진을 첨부하는 일도 많다. <br>
input의 타입에 file을 선언하고 name을 지정해주고 파일을 넘겨주면 된다. 클라이언트의 입장에선 어려울 것 없는 요청이다. 일반적으로 form데이터를 전송할 때 HTTP Header에는 기본적으로 application/x-www-form-urlencoded라는 정보로 노출된다. 
그러나 파일을 전송할 땐 enctype을 multipart/form-data로 지정해줘야 한다.

크기가 큰 파일을 여러 부분으로 조각내어 전송하는 방식이다.

