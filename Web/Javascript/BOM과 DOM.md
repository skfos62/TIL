# 07. BOM과 DOM

### 문서 객체 모델

**문서 객체 모델 (Document Object Model, DOM)**은 XHTML, HTML 문서용 api임.

DOM은 일종의 인터페이스로 해당하는 요소를 나타내는 노드, 노드의 속성을 나타내는 프로퍼티와 이를 조작할수 있는 여러 메서드를 담아 구조화한 객체로 표현.

DOM을 통해서 자바스크립트 같은 여러 프로그래밍 언어로 해당 요소에 접근해 구조나 내용, 스타일 변경 가능

DOM api는 Document를 통해 사용가능함.

### DOM트리

DOM은 문서를 노드의 계층적인 트리 구조로 나타냄. 노드는 서로 다른 특징, 데이터, 메소드를 가짐. 그리고 다른 노드와의 관계가 있을수도 있기에 계층을 생성하고 특정 노드에 뿌리를 둔 트리 노드로 표현됨.

최상위 노드를 root라고 하며 일반적으로 <html>이 root가 됨

```html
<!DOCTYPE html>
<html>
  <head>
    <title>DOM이란 무엇인가?</title>
  </head>
  <body>
    <p>문서 객체 모델입니다.</p>
  </body>
</html>
```

### Node

node 인터페이스는 전체 문서의 요소들에 대한 객체 형태의 기본 데이터 타입임. dom 트리의 구성요소를 나타 내며, nodeType, nodeName, nodeValue, 자식 노드와 형제 노드에 대한 관계 등 여러 정보를 갖음.

### Node의 계층 구조

```html
Event Target Node Text Commnet
<div>안녕</div>
<!- 주석 ->
Element SVGElement Html Element Html Input Element Html body Element Html Anchor
Element
<input type="" />
<body>
  <a href=""></a>
</body>
```

노드 인터페이스는 트리 형태의 계층 구조가 존재. 계층 구조의 가장 꼭대기에는 **eventTarget이 존재**

eventTarget은 이벤트가 발생했을때 대상이 되는 타깃을 의미

**Element**는 DOM 노드의 관계를 나타내는 기능들이 포함됨. **DOM 요소 생성의 가장 기본이 되는 클래스**로 **요소 탐색** (querySelector(), getElementByName() 등)의 **이벤트 리스너 관리** (addEventListener(), removeEventListener())등 여러 메소드를 제공.

HTMLElement는 html요소를 구현한 기본 클래스임.

### DOM 프로퍼티

DOM의 프로퍼티에 접근 할 경우 자바스크립트 객체 접근 방식과 동일하게 점 표기법과 괄호 표기법 사용가능

```html
<input type="checkbox" checked />

<script>
  const inputElem = document.querySelector("input");
  console.log(inputElem.checked);
  // ture
</script>
```

DOM 노드에 커스텀 프로퍼티를 할당하여 사용할수 있지만 다른 요소 사용시 혼란을 줄수있으므로 사용지양

### Node사이의 관계

최상위 노드를 제외한 모든 돔트리의 노드에는 각각 하나의 parent를 갖음. 또한 여러개의 child를 가짐

이러한 관계를 sibling이라고함.

DOM트리는 각 계층 관계를 프로퍼티로 제공하고, 각 노드를 탐색할 수 있음

- parentNode: 부모노드 반환
- childNodes: 요소의 자식 노드를 반환, NodeList로 반환
- firstChild: 자식 노드 중 첫번째 자식 반환, 없을경우 Null
- lastChild: 자식 노드 중 마지막 자식 반환, 없을경우 Null
- nextSibiling: 부모의 chiledNode중 자신 다음에 있는 노드 반환, 없을경우 null
- previousSibiling: 자신 이전에 있는 노드 반환, 없을 경우 null

```jsx
document.body.childNodes.forEach(node => {
  console.log(node); // Div, text, ...
});
```

위의 코드처럼 프로퍼티를 통해 전체 노드를 순회하기도 가능

### DOM Node 추가, 제거 하기

### createElement

document.createElement(tagName)을 호출 하면 tagName에 맞는 요소 노드 생성

```jsx
cosnt divElem = document.createElement('div') // <div></div>
cosnt ilElem = document.createElement('il') // <il></il>

ilElem.className = 'il-class-name' // class 속성 할당
ilElem.id = 'il-id' // id 속성 할당
ilElem.innerHTML = '<strong>삽입되는 요소 </strong>' // il 내부 html 노드 할당
```

### appendChild(), insertBefore()

새로운 노드 추가할때 씀. appenChild()는 부모노드의 마지막에 노드를 추가하는 메서드, insertBefore()는 특정 부모 노드 앞에 노를 삽입하는 메서드

두 메서드는 생성한 노드 뿐만 아니라 기존의 노드 위치도 이동시킬수있음.

insertBefore()dml ruddn parentNode, insertBefore(삽입하는 노드, 참조 노드)형태로 사용할수 있으며, 참조 노드 앞으로 이동.

참조 노드가 null 일경우 appenChild()와 동일하게 작동함

### append(), prepend(), after(), before()

- prepend(): 요소 내부의 가장 앞으로 이동
- append(): 요소 내부의 가장 마지막으로 이동
- before(): 요소 앞으로 이동
- after(): 요소 뒤로 이동

### 요소 검색하기

### getElementById()

요소의 id 속성을 이용해 document.getElementById(id)로 접근 할 수 있음. id는 문서에서 유일하기 때문에 요소를 찾을때 빠르게 찾기 가능. 일치하는 요소가 없을경우 null 반환

### querySelector()와 querySelectorAll()

querySelector()은 선택자(selector)과 일치하는 첫번째 요소를 반환함.

선택자는 css선택자와 동일하게 작성

**✅ querySelector()**

```jsx
const elem1 = document.querySelector("#test-id");
const elem2 = document.querySelector(".class-name");
const elem3 = document.querySelector(".class-name", "#test-id");
// or 조건으로 작성시 선택자에 일치하는 첫번째 요소를 반환
```

✅ **querySelectorAll()**

```jsx
// 선택자와 일치하는 모든 요소를 NodeList형태로 반환함
const elem1 = document.querySelectorAll("div");
console.log(elem1); // [DIV#test-id, DIV.class-name]
```

---

## DOM 이벤트

이벤트는 어떠한일이 발생했을때, 그 시점에 발생하는 신호를 의미.

이벤트 객체는 DOM 내에서 발생한 이벤트에 대한 정보를 담고있음, 이벤트 객체는 발생한 이벤트의 종류부터 요소에대한 정보, 캡쳐링 여부, 이벤트 발생 위치 등 여러 정보를 가지고 있음.

### target과 currentTarget

이벤트가 발생한 요소에 접근하고 싶은 경우, target 혹은 currentTaget프로퍼티를 통해 접근

- target: 이벤트가 처음 발생했던 대상 DOM 요소의 참조를 갖음
- currentTarget: 발생한 이벤트가 등록된 DOM요소의 참조를 갖음

### stopPropagation()과 preventDefault()

event객체를 제어하는 메소드임

- preventDefault() : 이벤트를 취소할 수 있는 경우 이벤트를 취소함. 대신 전파(캡쳐링, 버블링) 되는 이벤트는 막지 않으며 현재 이벤트의 기본동작만 중단
- stopPropagation(): 기본동작을 중단하지는 못하지만 이벤트가 전파되는것을 막음

[이벤트 참조 | MDN](https://developer.mozilla.org/ko/docs/Web/Events)

각종 이벤트 선택자 관련

## 이벤트 리스너 추가하기

### html 요소 속성으로 할당하기

on<event> 형태로 되어있는 속성에 리스너 할당가능

```jsx
<button onClick="alert('hello!')">alert띄우기 </button>
```

### DOM 프로퍼티로 할당하기

```jsx
<button id='button-id'> alert 띄우기 </button>
<script>
	document.getElementById('button-id').onClick = (event)=> {
		alert('hello world')
	}
</script>
```

onClick에 할당되는 리스너 함수는 이벤트 정보를 담은 event객체를 매개변수로 가짐. html의 요소 속성으로 이벤트 리스너를 등록하거나, DOM프로퍼티 관련해서 이벤트 리스너를 등록하는 방법은 동시에 여러 리스너를 등록할수없는 단점이 있음.

새로운 이벤트 리스너를 할당할 경우 기존값은 덮어씌워지기 때문에 마지막에 할당된 이벤트 리스너만 실행됨

### addEventListner 사용

addEventListner()는 앞서 사용한 방식과는 다르게 이벤트 타입에 여러개 리스너를 등록할수있으며, 버블링을 사용할지 캡처링을 사용할지 정밀제어가 가능.

가장 권장되는 방식

```jsx
<button id='button-id'> alert 띄우기 </button>
<script>
	// click으로 타입명시
	document.getElementById('button-id').addEventListner('cilck', (ev)=> {
		alert('hello')
}, true) // 캡쳐링 사용
</script>
```

할당한 이벤트를 해제하려면 removeEventListner()메서드를 사용. 해제하고 싶은 이벤트 리스터의 참조를 인자로 넘겨주면 이벤트가 해지됨

## 버블링과 캡쳐링

계층적 구조를 가진 DOM에 이벤트가 발생할 경우 연쇄적으로 이벤트가 전파(propagation)됨. 이벤트가 전파되는 방향에 따라서 버블링과 캡쳐링으로 구분됨.

### 버블링

DOM에 이벤트가 발생할 경우 부모 요소로 올라가며 차례대로 이벤트가 전파되는 흐름을 버블링이라고함.

대부분의 이벤트는 버블링을 기본 동작으로 갖음.

```jsx
<form id='form-id'>
	form 영역
		<div id='div-id'>
		div 영역
			<p id='p-id'>
				p영역
			<p>
		</div>
</form>

<script>
	const form = document.getElementById('form-id')
	const div = document.getElementById('form-id')
	const p = document.getElementById('form-id')

	[form, div, p].forEach(target => {
		target.addEventListner('click', () => {
			console.log(`${String(target.tagName)} 클릭!`) // 클릭한 DOM요소 이름 출력
		})
	})

</script>
// p클릭, div클릭, form클릭 순서대로 출력됨
```

p영역을 클릭할경우 등록된 이벤트 리스너가 실행되며 p클릭, div클릭, form클릭 순서대로 출력됨

### 캡쳐링

버블링과 반대로 상위 부모 요소로부터 자식 요소로 내려가며 이벤트가 전파되는것을 캡쳐링이라고 함.

```jsx
[form, div, p].forEach(target => {
  target.addEventListner("click", () => {
    console.log(`${String(target.tagName)} 클릭!`); // 클릭한 DOM요소 이름 출력
  });
}, true); // 캡쳐링 이벤트 핸들러
```

캡쳐링 이벤트 리스너를 등록한 후 동일하게 p영역을 클릭하면 버블링과는 반대로 상위 요소인 <form>부터 <div>, <p>의 순서대로 이벤트가 발생

표준 DOM의 이벤트는 캡처링, 타깃, 버블링의 흐름 순서를 갖음.

### 이벤트 위임

DOM 요소의 이벤트 흐름을 이요해 이벤트 제어를 상위 요소로 위임할수도있음

```jsx
<ul id="article">
  <li id="article-q">게시글1</li>
  <li id="article-q">게시글2</li>
  <li id="article-q">게시글3</li>
</ul>
```

위의 요소에 이벤트를 등록한다고 했을때 상위 요소에 이벤트를 위임하면

```jsx
const articleList = document.getElementById("article");

articleList.addEventListener("click", ev => {
  console.log(ev.target.id);
});
```

이런식으로 상위 요소에 이입트 를를위하며

이벤트 버블링을 통해 이벤트가 발생한 target요소로 이벤트가 전파됨.

---

## 브라우저 객체모델 (Browser Object Model, BOM)

웹 브라우저와 관련된 객체 모델 의미, 표준이 없고 브라우저별로 자유롭게 구현이 되어있음

### window객체

브라우저에서 제공하는 여러가지 기능들은 window 인터페이스를 통해 사용할수있음. window인터페이스는 일반적으로 두가지 의미가 있음.

- 브라우저 환경에서의 자바스크립트 전역객체를 의미. window객체를 통해 전역에 선언됨 변수나 함수에 접근할수있음.
- 브라우저창 (window를 의미) 창을 닫거나, 경고를 띄우고, 새창을 띄우는등의 메서드 제공

### History 객체

히스토리 객체는 현재 브라우저의 세션기록을 가지고 있음. window.history로 히스토리 객체에 접근 가능하며 사용자가 방문한 목록을 보는것 불가능.

현재 페이지 기준으로 세션 기록내의 앞 뒤로 이동하거나 특정위치로 이동할수있는 go()나 back(), pushState()같은 메서드 제공.

또한 세션에 연결할 상태를 저장하는 state객체 또한 존재

### forward(), back()

```jsx
history.forward(); // 세션기록 내의 앞 이동
history.back(); // 세션기록 내의 뒤 이동
```

### go()

현재 위치를 기준으로 오프셋에 해당하는 숫자를 넣어 위치 이동

```jsx
history.go(1); // forward()와 동일
history.go(-1); // back()와 동일
history.go(-2); // 현재 위치를 기준으로 뒤로 2페이지 이동
history.go(0); // 현재 페이지 새로고침
```

### pushState()

브라우저 세션기록에 상태 추가. state, title. url을 매개변수로 가짐. title은 아직 지원하지 않는 브라우저가 많아 빈 문자열 넘김. 새로운 url은 호출한 곳과 동일한 출처이여야한다.

```jsx
const state = { userId: 234 };
const title = "";
const url = "docs/1";

history.pushState(state, title, url);
```

pushState의 경우 새로운 Http 호출을 만들지 않아 페이지 존재 여부와 상관없이 추가됨.

### replaceState()

브라우저의 현재 세션기록을 인자로 넘어온 상태로 대체 사용법은 pushState와 동일

### popstate이벤트

popstate이벤트는 같은 문서에 대해 히스토리 엔트리가 변화할때 생기는 이벤트. 브러우저의 버튼을 눌러 페이지를 이동하거나 go(), back(), forward()와 같은 api 호출할때 발생

popstate는 이벤트 발생시 history.state에 정보가 담김

이러한 특징은 SPA의 라우터 구현할떄 사용됨. SPA환경에서 화면 간 이동시 전체화면을 다시 그리지 않아 빠른 전환이 가능하다.

## Location객체

로케이션 객체는 현재 페이지의 url, 프로토콜, hostname, 포트번호 등 위치에 관련한 정보를 포함함.

[Location - Web API | MDN](https://developer.mozilla.org/ko/docs/Web/API/Location)

로케이션 객체에 관한

### assign()

매개변수에 해당하는 url로 이동, history 스택에 추가되며, 뒤로가기 시 이전 페이지로 이동.

location.href에 url 지정시 이동하게 되는 이유 또한 assign()메서드가 호출되기 때문

### replace()

assign()과 동작은 동일하나 현재 페이지에 대한 history 스택이 초기화 된다는 점이다름

## navigator객체

navigator객체는 클라이언트의 id 및 상태를 나타내빈다. window.navigator나 navigator로 접근 가능함. 각 브라우저 마다 제공하는 정보가 다를수있음

- **appVersion**: 브라우저 버전 반환
- **platform**: 브라우저 실행되는 플랫폼 반환
- **vendor**: 브라우저 공급 업체를 나타냄
- **userAgent**: 사용자 에이전트가 담긴 문자열 반환. HTTP헤더와 navigator를 통해 접근 할수있음.
- serviceWorker: 서비스 워커는 브라우저가 백그라운드에서 실행하는 스크립트오 웹페이지와 별개로 작동. 푸시 알림 및 백그라운도 동기화와 같은 기능에 사용

### WebStorage

웹스토리지는 클라이언트 측에 이름:값 쌍의 데이터를 저장하는 두가지 메커니즘을 의미.

1. **localStorage**
2. **sesstionStorage**

브라우저에 따라 용량제한이 다를수 있지만, 대부분의 브라우저 모두 2MB이상의 데이터 저장 가능

[Web Storage API 사용하기 - Web API | MDN](https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)

값은 항상 문자열이며 다른타입의 값을 넣어도 문자열 형태로 변환되어 저장. 객체의 형태로 데이터를 저장하고 싶을경우 JSON.stringfy()와 JSON.parse()를 통해 저장가능

### localStorage와 sesstionStorage의 차이

- **localStorage:** origin이 같을 경우 여러 탭과 창에서 공유됨. 세션 이후에도 지속되는 저장소용으로 설계됨. 컴퓨터를 종료하거나 브라우저를 종료하더라도 지속됨
- sesstionStorage: 한탭에서 페이지의 세션이 유지되는 동안 origin별로 스토리지를 관리함. 페이지가 열려있는 동안이나 페이지 리로딩 혹은 복원시에는 데이터가 유지되지만, 다른세션이나 창이 종료될경우 데이터에 접근할수 없음

### storage 이벤트

WebStorage의 데이터가 변경될때 storage이벤트가 발생합니다.

stroage이벤트 객체에는 데이터에 대한 여러가지 정보가 담김

- **key:** 변경된 데이터의 키를 나타냄, clear()시 null 반환
- **oldValue, newValue:** 이전값과 새로운 값을 나타냄. 새로 추가가 되었을 경우 oldValue는 null이 되며 제거될경우 newValue는 null이 됨
- **url:** 문서의 URL을 나타냄
- **storageArea:** 갱신이 일어난 WebStroage객체를 나타냄
