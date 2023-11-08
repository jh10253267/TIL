# System.out.println을 쓰면 안되는 이유

## 개요
처음 자바에 입문하고 자바로 구현한 웹을 만들 때 System.out.println을 써서 내가 원하는 결과가 잘 나오는지 혹은 조건문에 따라 잘 나뉘는지 확인할 것이다.<br>
그러나 실무에서 System.out.println을 절대 사용하면 안된다고 하는데 그 이유를 알아보자

## println의 문제점

1. 로그로서의 기능을 수행하기 어렵다.
 로그는 에러가 발생한 상황을 기록하고 확인하여 문제를 진단하고 고치기 위해 사용된다. 하지만 System.out.println은 
 단순한 문자열 출력 메소드여서 날짜, 시각, 문제의 수준 등등을 따로 나타내지 않는다.

2. 로그 출력 레벨을 설정할 수 없다.
 로그레벨이 관리되지 않으면 프로덕션 환경에서도 불필요한 디버깅 정보가 출력되어 시스템의 보안에 문제가 생길 수 있다.
 


3. 성능 저하
 System.out.println 내부를 살펴보면 다음과 같이 구성되어있다.
 ```java
  public void println() {
        newLine();
    }
 ```
 ```java
     public void println(boolean x) {
        if (getClass() == PrintStream.class) {
            writeln(String.valueOf(x));
        } else {
            synchronized (this) {
                print(x);
                newLine();
            }
        }
 ```
 println은 newLine을 호출한다. 여기 synchronized 부분이 있는데 멀티 쓰레드 환경에서 한 쓰레드가 newLine메소드를 실행하면 메소드는 잠기게된다.<br> 다른 쓰레드는 이 쓰레드가 모두 사용하고 잠금을 풀어준 뒤에서야 newLine을 실행할 수 있다.<br><br>
 그래서 System.out.println은 콘솔에 출력이 될 때까지 애플리케이션이 멈추게 된다.<br> <br> 웹 특성상 다수의 사용자가 접속하기 때문에 다수의 사용자가 접속시 콘솔에 출력되기 전까지 웹 애플리케이션이 멈추게 된다.<br> 따라서 응답의 지연이 발생할 수 있다.