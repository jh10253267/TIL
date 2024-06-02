# File Upload

## 개요
 웹 어플리케이션에서 기본적인 파일 업로드와 다운로드를 하는 시나리오를 알아보자

## Multipart
웹 클라이언트가 요청을 보낼 때 HTTP 프롵토콜의 바디 부분에 데이터를 여러 부분으로 나눠서 보내는 것을 말한다.<br>
보통 파일을 전송할 때 사용한다.

폼을 처리할 때 인풋 타입을 파일로 주고 사용
일반적인 form 데이터를 전송 시에 HTTP Header에는 기본적으로, 'application/x-www-form-urlencoded' 라는 정보로 노출 된다. 이는 쉽게 말하자면 주소창에 `이름=값&이름=값`과 같은 형식으로 표현되듯이 인코딩 되는 것을 말한다. 그러나 파일은 표현 방식이 다르기 때문에 다음과 같은 헤더로 표현된다.

`Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7rKYKhIWaTrjvGi1`


## 사용방법

HttpServletRequest는 웹 클라이언트가 전달하는 Multipart데이터를 처리하는 메소드를 제공하지 않는다. 파일을 다루기 위해서는 별도의 라이브러리를 사용해야한다.

대표적인 라이브러리로는 아파치의 commons-fileupload가 있다.  
Spring MVC에서의 파일 업로드를 위해 몇 가지 라이브러리와 설정을 추가해주어야하는데 `commons-fileupload`, `commons-io`, `MultipartResolver Bean` 이런 것들이 필요하다.

디스패쳐서블릿은 준비과정에서 멀티파트가 요청으로 올경우 멀티파트 리졸버를 사용한다.

이를 위해 별도의 빈을 생성하는 코드를 작성해주어야한다.

프론트에서도 enctype을 multipart로 설정해주어야하고 메소드는 Post로, input의 타입을 file로, 만약 타입이 file인 input태그가 여러 개가 있고 name이 모두 같아면 서버에 file이라는 이름의 배열 형태로 전송되게된다.

이렇게 파일이 넘어오면 InputStream을 통해 파일을 서버에 저장하고 파일정보를 DB에 저장하면 된다.

```java
@PostMapping("/{reservationInfoId}/comments")
    public CommentResponse writeComment(@RequestParam("attachedImage") MultipartFile attachedImage,
                                        @RequestParam("comment")String comment,
                                        @RequestParam("productId")Integer productId,
                                        @RequestParam("score")Integer score) {
        try (
            FileOutputStream fos = new FileOutputStream("/tmp/" + attachedImage.getOriginalFilename());
            InputStream is = attachedImage.getInputStream();
        ) {
            int readCount = 0;
            byte [] buffer = new byte[1024];
            while((readCount= is.read(buffer)) != -1) {
                fos.write(buffer, 0, readCount);
            }
        } catch (Exception e) {
            throw new RuntimeException("file Save Error");
        }

        System.out.println(attachedImage.getOriginalFilename());
        System.out.println(attachedImage.getSize());
        return null;
    }
```

여기서 주의해야할 점은 웹의 사용자들은 굉장히 많을 것이고 업로드되는 파일도 굉장히 많을 것이다. 그러나 모든 파일의 이름이 각기 다를 것이라는 보장이 없다. 따라서 이름이 중복되는 일을 방지하기 위해서 파일 이름의 앞부분에 UUID(중복될 가능성이 굉장히 적은 값)을 붙여서 저장하는 방법을 생각해볼 수 있다.






