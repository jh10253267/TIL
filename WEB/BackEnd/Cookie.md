쿠키는 작은 정보의 뭉치로 클라이언트에 저장된다.
키와 값으로 구성된다.
하나의 쿠키는 4키로바이트로 제한되고 브라우저는 각각의 웹사이트당 20개의 제한을 두고있다. 모든 웹사이트를 합쳐서 최대 300개를 허용한다. 그러니 2.2mb까지의 쿠키를 저장할 수 있다.

javax.servlet.http.Cookie

서버에서 쿠키 생성, Reponse의 addCookie메소드를 이용해 클라이언트에게 전송

Cookie cookie = new Cookie(이름, 값);
response.addCookie(cookie);
쿠키는 (이름, 값)의 쌍 정보를 입력하여 생성합니다.
쿠키의 이름은 일반적으로 알파벳과 숫자, 언더바로 구성합니다. 정확한 정의를 알고 싶다면 RFC 6265(https://tools.ietf.org/html/rfc6265) 문서 [4.1.1 Syntax] 항목을 참조하세요.

클라이언트가 보낸 쿠키 정보 읽기

Cookie[] cookies = request.getCookies();
쿠키 값이 없으면 null이 반환됩니다.
Cookie가 가지고 있는 getName()과 getValue()메소드를 이용해서 원하는 쿠키정보를 찾아 사용합니다.

클라이언트에게 쿠키 삭제 요청

쿠키를 삭제하는 명령은 없고, maxAge가 0인 같은 이름의 쿠키를 전송한다. 이렇게 해서 쿠키를 만료시켜야하는 이유는 관리를 클라이언트에서 하기 때문.
Cookie cookie = new Cookie("이름", null);
cookie.setMaxAge(0);
response.addCookie(cookie);
 -> 중복을 허용하지 않는 것을 알 수 있다.

쿠키의 유효기간 설정

메소드 setMaxAge()
- 인자는 유효기간을 나타내는 초 단위의 정수형
- 만일 유효기간을 0으로 지정하면 쿠키의 삭제
- 음수를 지정하면 브라우저가 종료될 때 쿠키가 삭제
유효기간을 10분으로 지정하려면
- cookie.setMaxAge(10 * 60); //초 단위 : 10분
- 1주일로 지정하려면 (7*24*60*60)로 설정합니다.

javax.servlet.http.Cookie
Spring MVC에서의 Cookie 사용

@CookieValue 애노테이션 사용
- 컨트롤러 메소드의 파라미터에서 CookieValue애노테이션을 사용함으로써 원하는 쿠키정보를 파라미터 변수에 담아 사용할 수 있다.
컨트롤러에서 쉽게 읽어들일 수 있음.
컨트롤러메소드(@CookieValue(value="쿠키이름", required=false, defaultValue="기본값") String 변수명)


 