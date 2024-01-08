# Enum

## 개요

정해진 갯수만큼의 인스턴스를 미리 만들어둔다.

생성자가 있다 -> 객체를 생성해서 쓰겠다.

public 생성자는 없다. -> 객체를 내부에서 정해진 수만큼 생성한다.

선언부 밑에 수를 지정.

예를 들어 광역시를 enum으로 표현해보자

```java
public enum KoreanCity {
  SEOUL(11, 22), DEAGU(22, 33), BUSAN(33, 44)
  // 추가적으로 가져야하는 정보
  private double lat,lng;

  KoreanCity(double lat, double lng) {
    this.lat = lat;
    this.lng = lng;
  }
}
```

이런식으로 객체를 미리 만들어놓는 것이 enum이다.