# Next.js

## 개요

앞에서 공부해본 내용을 복습해보자면 리엑트는 Client Side Rendering 방식으로 앱을 구성한다. 그렇기 때문에 성능적인 이점이 생긴다. 그러나 단점도 존재하는데 바로 SEO 즉, 검색 엔진 최적화이다. CSR방식에선 첫 페이지에 빈 html파일을 가져와서 js파일을 해석하여 화면을 구성하기 때문에 포털 검색에 노출될 일이 많이 없다.

그래서 이를 SSR방식으로 구현할 수 있는 수단이 바로 Next.js이다. Next.js에선 Pre-Rendering을 통해서 페이지를 미리 렌더링 하며 완성된 HTML을 가져오기 때문에 사용자와 검색 엔진 크롤러에게 바로 렌더링 된 페이지를 전달할 수 있게 된다.

Next.js는 라이브러리가 아닌 프레임워크로 

## Let's get started!

### 설치 방법

```
npx create-next=app@latest
# or
yarn create next-app
```

```
npx create-next-app@latest --typescript
# or
yarn create next-app --typescript
```

