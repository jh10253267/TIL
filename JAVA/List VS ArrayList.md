# List VS ArrayList

```
List<String> list = new ArrayList<>();
ArrayList<String> list = new ArrayList<>();
``` 
둘 다 같은 동작을 수행할 수 있다.

## List와 ArrayList의 차이.

List는 인터페이스로 그 구현체가 ArrayList와 LinkedList 둘 중 어느것이 와도 사용할 수 있다.

ArrayList로 선언할 경우

```java
ArrayList<String> list = new ArrayList<>();
list = new LinkedList<>(); // error
```
이런 식으로 변경하지 못한다. 반면에<br>
List로 선언한 경우
```java
List<String> list = new ArrayList<>();
list = new LinkedList<>();
```
위와 같이 변경이 가능하다.

우선 List는 인터페이스로 구현체는 LinkedList나 ArrayList가 될 수 있다.
따라서 List로 선언하는 것이 다형성면에서 유리하게 된다.

참조하는 입장에서 List라는 인터페이스(경계면)를 바라본다. List의 구현체가 뭐가 오든 알지 못해도 기능에는 문제가 없다.(인터페이스의 본질)<br>
List형으로 선언하지 않을 이유가 없어보인다.<br>
만약 효율성의 이유 혹은 요구사항의 변경등의 이유로 ArrayList를 LinkedList로 변경하려 한다면 
구현체만 변경해주는 식으로 리펙토링을 할 수 있다.
