# Jacoco

## 개요

테스트에는 몇 개의 지표가 있다. 대표적으로 테스트 커버리지가 있다. 테스트 커버리지는 개발자가 작성한 테스트가 코드의 몇 퍼센트를 커버하는지를 나타내는 지표이다. 

만약 내가 작성한 테스트가 전체 코드 라인중 10%의 경로만 거쳐 테스트된다면 10%의 테스트 커버리지 값을 가지는 것이다.

## Jacoco?

그럼 테스트 커버리지를 어떻게 측정할까? Jacoco를 이용해 보는 방법이 있다. 

이름에서 알 수 있듯이 Jacoco는 Java의 테스트 커버리지를 체크하는 라이브러리이다. 테스트 코드를 돌리고 그 결과를 .html과 같은 형식으로 레포트를 생성해준다.

## 사용법

Jacoco를 사용하기 위해서는 build.gradle을 수정해줘야한다.

우선 플러그인 부분에 jacoco를 추가해준다.

```kotlin
plugins {
    ...
    id 'jacoco'
}
```

그런 다음 jacoco에 관련한 설정을 해줘야한다.

Jacoco Gradle 플러그인에는 두 가지 task가 있다.

* jacocoTestReport: 결과를 리포트 형식으로 저장한다.

* jacocoTestCoverageVerification : 내가 원하는 커버리지 기준에 만족하는지 확인해주는 설정을 작성한다. 만약 브랜치 커버리지를 일정 퍼센테이지 이상으로 설정하고 싶다면 이 곳에 정의해두면 된다.

나는 이렇게 정의했다. reports부분에는 원하는 형식의 레포트를 끄고 킬 수 있다. html 형식이 편하기 때문에 `html.required = true` 이렇게 설정했다.

`finalizedBy` 부분은 순서대로 task를 실행시키기 위한 설정이다. 예를 들어 test task가 실행되지 않았는데 jacocoTestReport task가 실행되면 레포트를 생성해주지 못한다.

그래서 일단 test task가 먼저 실행되어야하고 그 다음 jacocoTestReport task가 실행되어야하고 그 다음 jacocoTestCoverageVerification가 실행되어야한다.

```kotlin
jacocoTestReport {
    reports {
        xml.required = false
        csv.required = false
        html.required = true

        // 이런식으로 리포트 타입에 따라 저장경로를 다르게 설정할 수도 있다고 한다.
        //  html.destination file("$buildDir/jacocoHtml")
//  xml.destination file("$buildDir/jacoco.xml")

    }
    finalizedBy 'jacocoTestCoverageVerification'
}
```

test task에 finalizedBy를 설정해주어 순서대로 실행될 수 있게 설정해준다.

```kotlin
tasks.named('test') {
    useJUnitPlatform()

    finalizedBy 'jacocoTestReport'
}
```

그 다음은 jacocoTestCoverageVerification을 설정해준다.

설정한 minimun 커버리지를 넘지 못하면 테스트 실패가 난다.
```kotlin
jacocoTestCoverageVerification {
    violationRules { // 커버리지의 범위와 퍼센테이지를 설정합니다.
        rule {
            element = 'CLASS'

            limit {
                counter = 'BRANCH'
                value = 'COVEREDRATIO'
                minimum = 0.00
            }
        }
    }
}
```
`[ant:jacocoReport] Rule violated for class ... branches covered ratio is 0.04, but expected minimum is 0.60`

그리고 Querydsl을 사용하는 경우엔 Q도메인도 테스트 범위 내에 들어간다. 이는 테스트할 필요가 없는 부분이라 이 부분을 제외시킬 수 있다.

jacocoTestCoverageVerification에 다음과 같은 코드를 추가해준다.

```kotlin
jacocoTestCoverageVerification {
    def Qdomains = []
    // 패키지 + 클래스명
    for (qPattern in '*.QA'..'*.QZ') { // qPattern = '*.QA', '*.QB', ... '*.QZ'
        Qdomains.add(qPattern + '*')
    }
    violationRules { // 커버리지의 범위와 퍼센테이지를 설정합니다.
        rule {
            element = 'CLASS'

            limit {
                counter = 'BRANCH'
                value = 'COVEREDRATIO'
                minimum = 0.60
            }
            excludes = [] + Qdomains
        }
    }
}
// 출처 : https://bottom-to-top.tistory.com/36 
```

이렇게 하고 루트 디렉토리에서 build/reports/jacoco/test/html/index.html을 보면 테스트 커버리지가 측정되어 보여진다.

