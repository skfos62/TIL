자바스크립트는 1995년 넷스케이프 커뮤니케이션즈와 개발자인 브렌던 아이크가 개발한 프로그램 언어 

[](https://namu.wiki/w/JavaScript)

# 변수선언

---

### var

var 키워드를 사용해 변수를 선언하는 것은 ES 2015(ES 6)에서 let 과 const가 등장하기 전 까지 유일한 방법이였음. var로 선언된 변수는 기존에 선언된 변수의 값을 덮어쓰며, **함수 스코프를 기준으로 동작**함 

```jsx
var a = 1;

if (isSomething()){
	var a = 2;
}

console.log(a); // 2 

```

var로 변수를 선언할 경우 **스코프 내에 동일한 식별자가 가진 변수가 존재한다면 해당 변수 값에 재할당**함.

예제 처럼 특정한 조건에 따라 기존에 선언된 값을 덮어 쓴다면 디버깅도 힘들게 된다. 

블록 또는 함수에서 선언하지 않은 자바스크립트의 변수는 모두 전역 스코프를 기준으로 선언된것 즉 전역변수라고 부르고, 변수가 함수 스코프를 가진다는 것은 변수를 선언한 함수 몸체 안에서만 해당 변수를 접근할수 있다.

```jsx
function foo() {
	var a = 1;
	console.log(a);
}

console.log(a); // uncaught refenceError: a is not defined
```

❌ **안전한 값을 사용하지 않는 예제** 

```jsx
function foo() {
	for (var i = 1; i < 10; i ++ ) {
		// ...
	}
	console.log(i)	
}

foo()
```

위의 코드의 경우 foo() 안에 선언된 i 변수는 블록이 아닌 함수 스코프를 가지고 있기 때문에 함수 종료 전까지 접근할수있음. 이러한 접근은 불필요하며 혼란만 초래 또한 이미 i라는 변수가 선언되어 있다면 같이 덮어 씌워지므로 예상치 못한 문제 발생할것

 

### let과 const

let 과 cost는 ES2015에서 등장한 변수 선언 키워드임. **var와 다르게 재생산을 허용하지 않으**며 함수 스코프가 아닌 **블록스코프를 가짐** 

```jsx
let a = 1;
let a = 2; // SyntaxError 재선언을 시도하면 발생하는 에러 

{
	let a = 1;
}
console.log(a) // RegerenceError 
// 블록스코프를 가지기 때문에 해당 변수를 둘러싼 블록({})안에서만 변수에 접근 가능 
```

let (or const)는 재선언을 허용하지 않으며, for반복문의 블록스코프에 묶여 외부의 변수 값을 덮어쓰거나 불필요한 참조가 되는 문제를 막음. 또한 **반복문이 수행될때마다 지역변수로써 새로 선언** 되어 반복문 내에서 안전하게 값사용 가능 

**const 는 let과 달리 값변경을 허용하지 않음** 

### 객체와 타입

자바스크립트는 객체의 타입과 상관없이 var, let, const의 키워드로 변수 선언을함. 자바스크립트는 원시타입과 객체로 나뉨.

- [원시타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Data_structures#%EC%88%AB%EC%9E%90_%ED%83%80%EC%9E%85)
    - number, [string](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String), boolean, null, undefined, symbol, bigint
    - string: [이스케이프](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String#%EC%9D%B4%EC%8A%A4%EC%BC%80%EC%9D%B4%ED%94%84_%ED%91%9C%ED%98%84), [템플릿 리터럴](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)
- 원시타입을 제외한 나머지 모두 [객체 타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Data_structures#%EA%B0%9D%EC%B2%B4)

### 객체

원시타입이 아닌 모든값은 객체, [객체는](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object) **이름(키):값** 의 형태로 여러값을 포함하는 컨테이너

- 객체리터럴
    - 중괄호를 이용해 객체를 만드는 방법. 간단하게 사용할수있고 많이 사용되는 방법
        
        ```jsx
        const obj = {
        	id: 'id',
        	name: 'name',
        }
        ```
        

### 배열

[배열](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array)은 객체의 특별한 형태로 **순서가 있는 데이터의 집합**. 배열의 위치를 가리키는 인덱스로 각 원소에 접근 

**배열은 객체이지만 정수타입인 인덱스를 프로퍼티로 갖는 특별한 데이터** 

### 표현식 (Expression)

[표현식은](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators#expressions) **값으로 평가되는 구문**. 예를 들어 임의의 숫자나 문자열은 모두 표현식 

### 문 (Statement)

[문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements)은 일종의 지시를 내리는것. 문은 표현식 또는 다른는문을 조합해 동작을 수행하는 지시를 내림 

### 연산자 (Operator)

[연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators#operators)는 표현식과 표현식을 결합해 복잡한 표현식을 만듦. 표현식이 값으로 평가된다면 **연산자는 값을 만들어내는 방법**

- falsy한 값
    - **false, ‘’(빈문자열), null, 0, undefined, On, Nan**
- falsy한 값 외에는 모두 truthy한 값
- falsy한 값이라고 착각하는 truthy 값 **(참으로 평가되는것)**
    - **빈배열, 빈객체, 공백 또는 줄바꿈 문자열, 문자열 'false’**