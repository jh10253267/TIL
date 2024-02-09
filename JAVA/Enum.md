# Enum

## 개요

enum의 정의는 열거형이다. 그러나 열거형이라는 말을 들어도 이게 왜 필요하고 왜 나왔는지 별로 와닿지가 않는다. 

그래서 조금 더 풀어서 써보려고한다.

enum이 일반 클래스와 다른 점은 바로 생성자는 있지만 public 생성자가 없다는 점이다.

이 얘기는 객체를 만들어 쓰겠다는 것인데 외부에선 생성하지 못하고 내부에서만 생성할 수 있다는 얘기다. 

그래서 enum의 경우 선언부 밑에 객체 인스턴스의 수를 명시한다.

즉 enum은 정해진 갯수만큼의 인스턴스를 미리 만들어두고 사용하는 클래스라고 할 수 있다.

예를 들어 광역시를 enum으로 표현해보자

```java
public enum KoreanCity {
  // 이 것들은 객체이다.
  SEOUL(11, 22), DEAGU(22, 33), BUSAN(33, 44), DEJOEN(44,55), GANGJU(55, 66);
  // 추가적으로 가져야하는 정보(위도, 경도)
  private double lat,lng;

  KoreanCity(double lat, double lng) {
    this.lat = lat;
    this.lng = lng;
  }
}
```

이런식으로 객체를 미리 만들어놓는 것이 enum이다.

이렇게 한다면 언제든 필요할 때 KoreanCity.SEOUL과 같은 식으로 객체를 가져와서 사용할 수 있다.

이러한 특징을 이용하면 싱글톤 패턴을 구현할 수 있다. 싱글톤 패턴은 하나의 객체만 사용하는 것이기 때문에 생성할 객체의 수를 명시해두면 된다. INSTANCE; 그리고 객체를 사용할 때 SampleClass.INSTANCE 와 같이 사용할 수 있다.

