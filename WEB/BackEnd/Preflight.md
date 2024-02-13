# Preflight

웹사이트들을 이용하며 개발자도구를 열어서 네트워크 탭을 살펴보면 Preflight라는 항목을 볼 수 있다. 

Ajax는 GET/POST/HEAD와 같은 방식의 요청을 Simple Request라고 하고 여기서 Content-Type이 "application/x-www-form-urlcoded"(주소 끝에 쿼리스트링이 붙는 형태), "Multipart/form-data", "text/plain" 인 경우에는 Ajax요청을 허용한다. 그러나 커스텀 헤더를 이용하거나 Content-Type이 다른 경우에는 Preflight Request를 이용하여 사전 요청을 보낸다.