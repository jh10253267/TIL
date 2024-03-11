# JSON parse error: Cannot construct instance of

## 개요

로컬에서 테스트를 해보다가 에러를 발견했다. 에러 문구는
```
JSON parse error: Cannot construct instance of 
```
api 테스트 툴로 서버에 데이터를 보냈는데  자바의 객체와 json 상호 변환 동작에서 에러가 발생했다.

json을 자바 객체로 매핑시킬 때 일단 기본 생성자를 만들고 setter로 데이터를 할당한다고 한다.(실험해볼 예정)

해당 객체를 가보니 기본 생성자가 없어서 추가해주었더니 해결되었다.