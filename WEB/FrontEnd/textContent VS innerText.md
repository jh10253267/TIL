# testContent VS innerText

## 개요

웹 프로젝트를 하다 보면 뷰 화면에서 동적으로 내용을 바꾸어야할 때가 있다. 이럴 때 사용하는 속성이 바로  textContent와 innerText이다. 모두 꺽쇠로 구성되어있는 html 태그 안의 텍스트를 가져오고 등호를 사용하여 새롭게 할당하는 속성이다. 이렇게 내용을 바꾸면 출력결과는 동일하다. 그렇다면 어떤 이유로 다른 하나가 없어지지 않고 남아있을까?

## 차이점

textContent는 내용의 줄바꿈 까지도 다룬다. html의 특성상 태그 내부의 내용의 띄어쓰기를 한 칸까지만 인식한다. 그 이상으로 띄어쓰기를 하더라도 띄어쓰기 한 칸으로 인식한다.

textContent는 한 칸 이상의 띄어쓰기까지 인식하여 다루고 innerText는 실제 화면에 보여지는 내용만 다룬다.

또 다른 차이는 textContent는 텍스트 노드가 대상이 되지만 innerText는 화면에 보여지는 텍스트 노드만 대상이 된다는 점이다.

# 그렇다면?

뷰의 내용을 동적으로 바꾸려면 대상은 화면에 보이는 텍스트이기 때문에 innerText를 사용하는 게 좋을 것 같고 아직 textContent의 필요성은 못느끼겠다.