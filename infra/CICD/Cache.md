# Cache

## 개요

Github Action이 작동하면 gradle이 빌드된다. 그러나 이전에 빌드 결과를 저장하고 이를 재활용할 수 있는 방법은 없을까?

Gradle은 빌드할 때 의존성 패키지들을 모두 다운받는다. 이 때 Gradle은 빌드 시간과 네트워크 통신을 줄이기 위해 의존성 패키지를 캐싱해서 재사용하는 방법을 사용한다.

Github Action의 workflow는 실행시 새로운 환경을 구축하고 새롭게 의존성 패키지들을 가지고 와야한다. 그리고 테스트가 늘어날 시 빌드 체크 시간은 더 늘어난다.

빠른 주기의 개발및 배포가 일어나기 위해선 이와 같은 시간을 줄이는 것도 중요할 것이다.

이를 해결하기 위해서 Github Actions의 actions/cache를 사용해볼 수 있다.

## 방법

cache action은 github actions에서 지원하고 있다.

캐싱을 정의하는 것은 다른 스탭을 정의하는 것과 비슷하다.

```yaml
 - name: Cache Action
      uses: actions/cache@v3
        with:
          path: |
          ~/.gradle/caches
          ~/.gradle/wrapper
          key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle', '**/gradle-wrapper.properties') }}
          restore-keys: |
          ${{ runner.os }}-gradle-
```
path: 캐시의 저장과 복원에 사용되는 runner내부의 파일 경로이다.
key: 캐시 저장, 복원에 사용되는 키이다.

restore-keys: 내가 설정한 key로 cache miss가 발생할 때 사용할 수 있는 후보 키이다.

해당 경로의 파일들은 실행 os가 변경되었을 때 다시 설정되어야 하므로 runner.os를 key에도 추가해주고 .gradle파일 혹은 gradle-wrapper.properties가 변경되었을 때도 해당 경로의 파일들이 다시 설정되어야하기 때문에 hashFiles메소드를 통해 해당 파일들로 해시값을 설정해 키로 만드는 작업이 필요하다.


실제 적용해보고 성능을 비교해보자

캐싱을 사용하지 않았을 때 빌드 속도
<img width="1031" alt="스크린샷 2024-03-16 시간: 17 05 30" src="https://github.com/jh10253267/TIL/assets/108499717/e1f8d7a3-6585-4cc2-a36b-5f1e75c092d2">

캐싱을 사용했을 때 빌드 속도
<img width="1009" alt="스크린샷 2024-03-16 시간: 17 05 56" src="https://github.com/jh10253267/TIL/assets/108499717/0d84ed5a-da60-458e-8028-bfe102d880d1">

<img width="1102" alt="스크린샷 2024-03-16 시간: 17 09 35" src="https://github.com/jh10253267/TIL/assets/108499717/e6e5fbe3-c936-4843-8e64-b45c66ca562e">

평균 44.75s가 걸리던 빌드 속도가 20s로 단축되었다. 약 54%가 단축된 것이다.