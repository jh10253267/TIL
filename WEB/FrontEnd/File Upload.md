# File Upload

## 개요

우리는 프론트 단에서 정보를 보낼 때 form태그와 input을 이용한다. form태그의 속성에 경로와 메소드를 명시해주고 submit을 하면 된다. 근데 우리는 글이나 숫자를 양식에 맞게 작성한 뒤 제출하기도 하지만 리뷰 등록과 같이 이미지 파일을 업로드하거나 원서 접수를 할 때 사진을 첨부하는 일도 많다. <br>
이미지의 경우에도 input의 타입에 file을 선언하고 name을 지정해주고 파일을 넘겨주면 된다. 

## 방법

```html
<input type="file" name="postImage" id="postImage" accept="image/*">
```

name 속성은 클라이언트와 서버간의 통신을 위한 이름을 적어주면 되고 accept는 허용 가능한 파일 확장자 타입을 지정해줄 수 있다.

 일반적으로 form데이터를 전송할 때 HTTP Header에는 기본적으로 application/x-www-form-urlencoded라는 정보로 노출된다. 

 이는 웹에서 폼 데이터를 인코딩하는 방식 중 하나로 name이 John이고 age가 28이라면 인코딩된 문자열은 다음과 같다.
 ```
 name=John&age=20
 ```
 그러나 파일의 경우 크기가 일반적인 경우보다 크기때문에 하나의 파일을 잘게 쪼개서 전송한다.  
이러한 인코딩 방식은 enctype을 multipart/form-data로 지정해줘야 한다.

## 유효성 검사

파일을 업로드할 때 서버에서 규정한 크기 이상의 파일이라던지 확장자가 잘못되었다던지 체크해야할 필요가 있다.

우선 아까 html태그애서 봤던 것처럼 accept를 사용하여 체크할 수 있다.

이 방법 외에도 자바스크립트에서 체크하는 방법이 있다.

change이벤트는 사용자가 입력 폼 요소의 값을 변경할 때 발생한다.

```js
const elImage = document.querySelector.("#postImage");
	elImage.addEventListener("change", (evt) => {
		const image = evt.target.files[0];
		if(!validateImage(image)) { 
			return;
		}
	})
```

```js
function validateImage(image) {
	const result = ([ 'image/jpeg',
					  'image/png',
					  'image/jpg' ].indexOf(image.type) > -1);
	return result;
}
```

이렇게 자바스크립트를 이용하여 타입을 체크할 수 있다.

우리가 웹사이트를 이용하며 사진을 업로드할 때를 생각해보면 별도로 submit버튼을 눌러서 사진을 제출하지 않았는데 내 사진이 썸네일로 변환되어 보이는 것을 볼 수 있다.

이는 위와 같이 change이벤트를 이용하여 검사가 유효성 검사에 통과한 뒤 Ajax요청을 보내서 서버에서는 이를 썸네일로 만들고 우리에게 보여주는 것이다.  
이 방식은 사용자가 업로드한 이미지가 어떤 건지 사용자에게 인지시키려는 의도이다.

좀 더 간편하게 미리보기 이미지를 만들 수 있는 방법이 있는데 바로 `createObjectURL`를 사용하는 것이다.

이 메소드는 정적 메소드로 주어진 객체를 가리키는 URL을 DOMString으로 반환한다. 해당 URL은 document가 사라지면 함께 무효화된다.


