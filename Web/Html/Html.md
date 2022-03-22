## Html

- html은 `hyper text markup language` 의 약자로 마크업 구성에 가장 많이 사용되는 언어.
- W3C와 WHATWG의 표준을 따르고 있음
- 다른 언어들 보다 느슨한 문법을 가지고 있음

### 인라인(inline) 요소

- 태그가 할당된 텍스트나 이미지의 크기에 맞는 `필요한 공간만 차지`함. 높이 너비를 지정할 수 없으며, 줄 내부 어디서든 시작됨

### 블록(block) 요소

- 태그가 시작되면 `이전 요소와 상관없이 개행해서 새로운 줄에서 시작`됨. 너비는 좌우 양쪽 부모요소의 100%차지 왼쪽에서부터 오른쪽으로 확장됨 (ex. <div>)

### html의 기본 골격 코드

```html
<!-- 문서의 타입 지정 -->
<!DOCTYPE html>
<!-- 문서의 루트 지점을 명시하는 태그 lang속성을 해당 서비스에 맞게 넣어주는것이 중요 -->
<html lang="ko">
	<!--문서의 제목과 인코딩 형식을 지정-->
	<head>
		<title> 문서의 제목입니다	</title>
		<!-- 메타데이터(데이터에 대한 데이터, 어떤 목적으로 만들어진 데이터) 작성-->
		<!-- 주로 기계가 읽고 이해하는 정보, 인코딩 방식, 뷰포트 지정 등등 수행 -->
		<!-- charset은 문서의 인코딩 방식 지정 -->
		<meta charset = "UTF-8"> 
		<!-- http-equiv은 ie의 렌더링 사양을 세밀하게 조정-->
		<meta http-equiv="X-UA-Compatible" content="IE=Edge" /> 
	</head>
	<body>
		<!--문서의 내용 -->
	</body>
</html>
```

### 시맨틱(sementic)

- html은 **시맨틱 하게 작성**해야한다.
- **시맨틱**이란? **의미에 맞게 태그를 사용**해 문서를 작성하는일
    ```html
    <h1~h6>, <header>, <footer>, <main>, <article>, <section>, <aside>, <nav>
    ```
    - 위의 태그들의 쓰임새를 공부해야함

### SEO(search engine optimization)

- 우리의 사이트를 찾기 쉽도록 개선하는 여러 노력을 **검색 엔진 최적화(seo)**라고 부름
    1. 시맨틱하게 html을 작성해야함. 
    2. <title>을 놓치지 말고 적절하게 작성하자.
    3. <meta name = “description”>을 활용해 페이지 설명을 남기자.
    4. <meta charset=”UTP-8”/>을 사용해 인코딩 방식을 지정하자.
    5. open graph, twitter 태그를 사용해 외부 사용자를 유인하자.
        1. **활용 예제** 
        
        ```html
        <meta property="og:title" content="페이지 이름"/>
        <meta property="og:description" content="페이지에 대한 간략한 한두줄 설명"/>
        ```
        
        [Open Graph protocol](https://ogp.me/)
        