# Social Login

## 개요
언제부턴가 쇼핑몰을 이용하거나 어떤 서비스를 이용할 때 구글로 로그인, 카카오 계정으로 로그인 등과 같이 우리가 많이 쓰는 계정으로 전혀 다른 서비스에 로그인하고 이용할 수 있게 되었다.

이렇게 우리는 서비스마다 다른 양식으로 회원가입하고 그 때 사용했던 아이디와 비밀번호를 기억하거나 저장하는 불편함없이 서로 다른 서비스를 이용할 수 있게 되었고 우리의 불편함은 많이 해결되었다. 이것은 Social Login덕분에 가능한 것이다. 이러한 방식을 OAuth라고 한다.

## 소셜 로그인 작동 원리
대부분의 소셜 로그인은 OAuth2라는 방식을 이용해서 데이터를 주고받아 사용자의 정보를 전달하는 방식으로 처리된다.

이는 문자열로 구성된 토큰을 주고받는 방식으로 토큰을 발행하거나 검사하는 방식을 통해서 서비스간 데이터를 교환한다.

스프링 부트에선 이런 소셜 로그인을 쉽게 이용할 수 있게 라이브러리를 제공하고 있어서 개발 시간을 많이 단축할 수 있다.
그중에서 쉽게 사용가능한 카카오 소셜 로그인 서비스를 설명해볼까한다.

![소셜 로그인 이미지](https://2dowon.github.io/assets/images/react/login2.png)

그림을 보면 우선 서버에 요청을 보내 인가코드를 받는다. 이는   놀이공원에서 입장권을 구매하는 것과 같다.

그런 다음 인가코드는 사용자가 카카오 서비스에 등록한 url로 전달된다.

그리고 이 인가코드는 자신의 비밀키와 같이 이용되어 토큰을 생성할 때 사용된다.

인증 토큰은 놀이공원에서 특정한 놀이 기구를 타기위한 승차권과 같다. 즉 원하는 데이터에 접근할 수 있는 권한이다.

 이렇게 얻어온 토큰을 이용해서 카카오에 사용자의 정보를 요청한다. 이 때 사용자의 아이디와 이메일주소 등을 얻어와 서버에서 사용할 수 있게 된다.

 처음에는 이게 어떻게 로그인으로써의 기능을 할 수 있는지 궁금했다. 그 이유는 카카오쪽에서 사용자의 정보를 받아온다고 해도 이 것은 아무 의미없는 것일 수 있다. 카카오에서 전달받을 수 있는 정보는 그저 유저의 아이디와 이메일뿐이기 때문이다.

 그래서 고민을 했고 정리해본 시나리오는 다음과 같다.

 카카오에서 받은 정보를 통해 우리 서비스에 등록이 되어있는 사용자인지부터 확인해야한다. 여기서 두가지 경우가 있는데 사용자의 정보가 우리 서비스에 등록되어있는 경우와 우리 서비스를 처음 이용하는 사용자가 카카오 로그인을 통해 로그인한 경우이다.

일단 카카오에서 받아온 유저정보를 이용해 우리 서비스의 db에서 조회해봐야한다. 만약에 존재하는 사용자라면 우리 서비스에서 통용되는 토큰을 발급해준다.

만약 처음 이용하는 사용자라면 회원가입을 진행하면 된다.
사용자의 아이디와 이메일정보를 가지고 db에 등록하고 그 정보를 바탕으로 토큰을 생성해준다.

소셜로그인은 이렇게 진행된다.

## 구현해야할 것들

 우선 사용자가 소셜 로그인을 시도할 수 있어야한다. 이 부분은 카카오에서 제공한다. 로그인 페이지에서 카카오 로그인 버튼 혹은 링크를 만들고 사용자가 클릭하면 카카오에서 카카오 로그인 ui를 제공해준다.

 따라서 로그인 페이지와 카카오 로그인을 시도할 수 있는 링크가 필요하다.

 그리고 이렇게 로그인한 정보는 개발자가 미리 지정해둔 url로 쿼리 스트링을 포함하여 전송된다. 
 ```url?code=xxxxxx```
 
 url로 넘어온 인증 코드를 파싱해야하기에 이러한 url을 매핑하는 컨트롤러를 만들어줘야한다.

 인증 코드를 받은 다음엔 관련된 로직을 구현해줘야한다.
 로직은 서비스단에 구현해준다.

 필요한 로직은 다음과 같다.

 인증 코드를 받은 뒤 이를 이용해 토큰을 요청한다.
 받아온 토큰으로 카카오에 해당 유저의 정보를 요청한다.
 유저의 정보를 우리 서비스에서 조회한다. 
 유저의 정보가 있다면 카카오에서 얻어온 정보를 갱신해준다.
 유저의 정보가 없다면 회원가입을 진행하고 로그인처리를 해준다.


 






