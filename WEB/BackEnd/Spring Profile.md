# Spring Profile

## 개요

Spring에서는 어플리케이션 설정을 특정 환경에서만 적용되게 하거나 환경별로 다르게 적용할 때 이러한 설정을 나누어 관리할 수 있는 Spring Profile을 제공한다.

## 실습

우선 하나의 yml파일을 `---`로 구분하면 서로 다른 파일을 한 파일에 작성한 것처럼 작동한다.

```yml
spring:
  profiles:
    active: local

---
spring:
  config:
    activate: 
      on-profile: common
---
spring:
  config:
    activate: 
      on-profile: Prod
```

여러개의 설정을 같이 묶어서 적용시킬 수도 있다.
```
spring:
  config:
    activate: 
    group:
      local:
        common
      prod:
        common
```

이렇게 설정을 하고 프로젝트를 실행시켜보면...

```bash
The following 2 profiles are active: "local", "common"
```

잘 적용되는 것을 볼 수 있다.