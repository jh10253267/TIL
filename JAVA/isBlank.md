# isBlank

## 개요

프로젝트를 하던 중 스웨거에서 patch 메소드에 맞게 작성되어있는 코드를 테스트하다 문제가 생겼다.

클라이언트에서 해당 필드를 수정하고 싶지 않아 빼고 보내는 경우 null이 아닌 필드만 수정하도록 하는 로직이 있었는데 NullPointException이 생겼다.

이번 기회에 자세히 알아보려한다.

## isBlank

isBlank 메소드는 java.lang.String에 있는 메소드이다.

구현 코드는 다음과 같다.
```java
public boolean isBlank() {
        return indexOfNonWhitespace() == length();
    }
private int indexOfNonWhitespace() {
        return isLatin1() ? StringLatin1.indexOfNonWhitespace(value)
                          : StringUTF16.indexOfNonWhitespace(value);
    }    
```

isBlank는 witespace가 아닌 문자의 인덱스와 문자열의 길이를 기교한다.

만일 빈 문자로만 이루어져있는 경우엔 `nonWhitespace() == length()`가 true이고

문자열이 비어있다면 `nonWhitespace() = 0 length() = 0`가 true가 된다.

이 두 경우를 체크하여 빈 문자로만 이루어져있는 경우와 문자열이 비어있는 경우엔 isBlank가 ture를 리턴한다.

널체크를 위한 메소드가 아니라 메소드의 이름 그대로 문자열이 비어있는지를 체크하는 메소드인 것이다.

따라서 null인 필드에 접근해서 체크를 하니 NullPointException이 던져진 것이였다.
