# form 데이터 유효성 검사

## 개요

웹 사이트에 회원가입을 할 때를 생각해보면 입력 폼에 사용하고자 하는 아이디와 비밀번호, 그리고 다른 추가 정보들을 입력하고 제출하기 버튼을 누른다.

이렇게 작성한 데이터는 서버로 전송된다.

만약 사이트에서 요구하는 입력 형식을 맞추지 않는다면 서버에서 체크하고 오류를 반환할 것이다.

예를 들어 아이디는 8자에서 12자 사이의 영소문자와 숫자로 구성되어야한다던가 이메일을 입력하여 전송하는데 이메일의 형식이 맞지 않는다던지 하는 경우가 있을 것이다.

이메일 형식 검증은 input태그의 type에 email을 선택하면 자동으로 검사를 해주지만 일반 텍스트 값을 입력해야하거나 한다면 무심결에 작성한 입력값이 형식과 맞지 않을 경우 서버까지 요청이 접수된 다음에 내기 입력한 값이 틀렸다는 걸 안다면 유저 입장에서는 답답함이 들 것이다.

그리고 화면이 이동될테니 입력값도 다 지워져서 다시 입력해야하는 경우 사이트를 이용하지 않을 것이다.

이런 경우는 다들 한 번씩은 겪어본 적이 있다.

프론트에서도 미리 입력값을 검사하여 빠르게 유저에게 안내해줄 수 있는 것이 좋다.

## 구현 방법

우선 form 형식이 있고 그 내부에 type이 submit인 input이 있는 경우 유효성 검사를 할 틈도 없이 서버에 전송된다.

이런 기본 동작을 막기 위해 function의 인자로 event를 넣어주고 `event.preventDefault();`를 적용해준다.

```html
<form action="/join" method="post" id="myform">
                <div class="inputWrap">
                    <div class="email">
                        <span> Email </span> <input type="text" name="email"><br/>
                    </div>
                    <div class="password">
                        <span> Password </span> <input type="password" name="password"><br/>
                    </div>
                </div>
                <input class="sendbtn" type="submit">
           </form>
<script>
let btn = document.querySelector(".sendbtn");
let result = document.querySelector(".result");

btn.addEventListener('click', function(event) {
  event.preventDefault();
  var emailValue = document.querySelector("[name='email']").value;
		    var bValid = (/^[\w+_]\w+@\w+\.\w+$/).test(emailValue);
  if(isValid) {
    alert("정보를 정확히 입력해주세요.");
  } else {
    document.getElementById('myform').submit();
  }
});
</script>
```

위에서는 `click`이벤트를 청취하고 있지만 `submit`을 청취하는 것도 가능하다.

또 다른 방법도 있는데 입력 창에 커서를 올려놓고 입력을 하기 시작하면 실시간으로 입력값을 체크하여 안내를 해주는 사이트도 있다.

이는 change이벤트를 사용하는 것으로 구현이 가능하다.



