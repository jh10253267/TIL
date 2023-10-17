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
이런 식으로 변경하지 못한다. 반면에
```java
List<String> list = new ArrayList<>();
list = new LinkedList<>();
```
list인터페이스형으로 선언할 경우 다음과 같이 변경이 가능하다.


우선 List는 인터페이스로 구현체는 LinkedList나 ArrayList가 될 수 있다.
따라서 List로 선언하는 것이 다형성면에서 유리하게 된다.

만약 효율성의 이유 혹은 요구사항의 변경등의 이유로 ArrayList를 LinkedList로 변경하려 한다면 
구현체만 변경해주는 식으로 리펙토링을 할 수 있다는 이점이 있다.
