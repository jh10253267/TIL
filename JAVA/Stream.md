# Stream

## 개요

어떤 기능이 많이 쓰인다는 것은 이전의 방식보다 더욱 편리하고 배우기도 어렵지 않고 어렵다고 하더라도 충분히 배울만한 가치가 있다는 의미이다.

그러나 스트림을 검색했을 때 나오는 글들은 복잡하고 어렵다. 새로운 기술을 쓸 메리츠가 충분해서 사용하는 것인데 기존의 불편하고 이해하기 어렵고 많은 것을 알아야한다면 쓸 이유가 없다. 

그래서 우리의 코드에서 어떻게 쉽고 빠르게 스트림을 익히고 사용할 수 있는지를 간단한 예제를 통해 알아보려한다. 


## 예제

### IntStream

어떤 데이터를 정해진 수 만큼 만들고 싶을 때

테스트용 더미데이터를 만들고 insert하는 예제

```java
IntStream.rangeClosed(1, 100).forEach(i -> {
  UserDTO userDTO = UserDTO.builder()
    .username("user"+i)
    .password("1111")
    .build();

  userRepository.save(userDTO);
});
```
위와 같이 사용할 수 있다. 기존의 for문을 사용하듯이 쉽게 익히고 사용할 수 있다.









