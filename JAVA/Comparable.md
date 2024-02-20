# Comparable

## Equals

String a = "test";
String b = "test";

두 문자열을 비교해보자
a.equals(b)

위의 결과는 같은 문자열을 비교했으니 참이다.

Object a = "test";
Object b = "test";

a.equals(b)

위의 결과는 두 객체의 메모리 주소를 비교하기 때문에 일치하지 리턴값은 false가 된다.

그렇기 때문에 문자열을 비교하려면 문자열 클래스의 equals를 오버라이딩 해서 사용해야한다.

## Comparable

Comparable은 인터페이스로 객체에서 원하는 자료형으로 비교하기 위해 사용한다. 이 인터페이스는 compareTo메소드만 가지고 있다.

같은 자료형의 다른 객체 하나를 인자로 받아와 비교하는 compareTo메소드를 사용한다.

public int compareTo(T obj);

여기서 T는 비교할 객체의 타입이다. 

Comparable 인터페이스를 이용해 커스텀 정렬도 가능하다. 코테를 풀 때 문제에서 요구하는 대로 정렬을 해야할 때 사용할 수 있다.