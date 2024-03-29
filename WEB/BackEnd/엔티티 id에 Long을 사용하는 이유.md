# 엔티티의 id에 Long을 사용하는 이유

우선 엔티티의 id의 타입으로 Integer가 아니라 Long을 사용하는 이유가 뭘까?

엔티티에서 id라고 하면 PK값을 의미한다. PK는 기본키로 테이블 내에서 유일하게 식별이 가능해야한다. 그게 기본키의 정의이다.

그래서 id값을 정할 때 Auto_increment라는 전략을 사용하곤하는데 이는 테이블에 데이터가 생성될 때마다 하나씩 증가하는 정수를 사용하는 것이다. 예를 들어 첫 번째 등록된 튜플은 1이라는 아이디를 갖고 그 다음 등록된 튜플은 2라는 아이디를 갖는식이다. 그렇다면 Integer만으로도 충분한 것 아닌가 싶은 생각이 들 수 있다. SQL의 int 타입은 최대 2,147,483,647까지 저장이 가능하다.

데이터베이스에 이 만큼의 큰 데이터가 저장되어있는 일이 없을지라도 Auto_increment의 방식을 생각해보면 더 큰 자료형이 좋다. Auto_increment는 무조건 1씩 증가한다. 이전의 데이터가 삭제되더라도 고려하지 않고 새로운 데이터가 들어오면 무조건 이전의 id값에서 1이 증가되어 저장된다.

그러니 저장되어있는 데이터와 상관없이 삽입 삭제가 빈번하게 일어난다면 Long 타입으로 설정하는 것이 유리할 것이다.

그래서 Long타입을 선언하는 편이 추후에 불편한 추가 작업을 하지 않을 수 있다는 장점이 있다.

그러나... 만약 토이 프로젝트에서 Integer 대신 Long 타입을 사용했다면 명확한 이유를 대기 힘들 수 있다. 나는 관습적으로 사용했다고 밖에 말을 못할 것 같다.

자바에서 엔티티의 id로 Long 타입을 사용하는 이유는 이렇다.

long(원시형)의 경우 null이 올 수 없지만 Long 타입의 경우 null이 올 수 있다. 영속성 컨텍스트의 작동 방식을 생각해보면 insert의 경우 먼저 데이터베이스에 삽입을 한뒤 나온 pk값을 객체에 할당한다. 그렇다면 자바의 객체는 이미 만들어진 후 pk값을 할당해주어야한다는 말인데 그렇기 때문에 null이 올 수 있는 Long 값을 사용하는 것이다.




