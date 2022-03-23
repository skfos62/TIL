자바스크립트는 느슨한 언어이기 때문에 관대한 타입변환을 수행함. 그래서 타입변환 규칙을 ECMAscript에 나와있는 명세로 공부하는것이 좋음 

---

## 타입변환

자바스크립트에서 타입변환은 **명시적 강제 변환**, **암시적 강제 변환** 두가지가 있음 

### 명시적 강제 변환

명확하게 의도를 갖고 타입을 변환하는것

- 문자열로 변환
    - **String()**
        
        ```jsx
        console.log(String(3)) // '3'
        ```
        
    - **toString()**
        
        ```jsx
        const num = 4;
        console.log(num.toString()) // '4'
        ```
        
- **String()과 toString()의 차이점**
    - String()과 toString()은 대부분 동일한 결과를 반환하지만 null과 undefined의 경우 그렇지 않음
        
        ```jsx
        console.log(String(null)) // 'null'
        console.log(String(undefined)) // 'undefined'
        
        undefined.toString(); // type error 발생
        null.toString(); // type error 발생
        ```
        
        null과 undefined의 타입은 값이 비어있음, 할당되지 않은상태를 나타내는 원시타입이므로 toString()을 호출할수있다면 논리적으로 맞지않음. 
        
        이 타입들은 객체가 아니기에 사용할수 있는 프로퍼티가 없는것이 맞음.
        
        반면 String()은 심볼 이외의 모든 타입이 ECMAScript에 있는 ToString추상 연산 규칙을 따르기 때문에 문자열로 명시적 강제 변환을 하고 싶은 경우 String()이 적합 
        

- 숫자로 변환
    - Number()
        
        ```jsx
        console.log(Number('3')) // 3
        console.log(Number(true)) // 1
        console.log(Number(null)) // 0
        ```
        
    - parseInt()
    **parseInt()는 문자열만 대상으로 변환**. 값이 문자열이 아닐경우 해당 값을 문자열로 변환한 후에 사용
        
        ```jsx
        console.log(parseInt('10',10)) // 10
        console.log(parseInt('-1',10)) // -1
        
        // 두번째 인자는 '기수'임. 기수를 생각하면 첫번째 인자를 기준으로 
        // 추정하여 변환하므로 의도치않은 결과가 나올수 있음 
        ```
        
    - **Number()와 parseInt()의 차이점**
        - Number()의 경우 숫자로 변경이 불가능한 문자가 있으면 NaN으로 반환
        - parseInt()의 경우 변경 불가능한 문자가 나올때까지 최대한 숫자로 변환하여 결과 반환
    
    [ECMAScript® 2021 Language Specification](https://262.ecma-international.org/12.0/#sec-type-conversion)
    
    타입 변환 관련 ECMAScript 명세서
    

### 암시적 강제 변환

연산 중에서 내부적으로 타입을 변환하는것 

- 덧셈 연산자
- 동등연산자
- 비교연산자
- 조건 표현식과 논리연산자
    
    조건 표현식은 암시적 강제 변환을 아주 흔하게 사용**. 모든값은 불리언으로 변환되어 조건 표현식에서 평가됨**
    
    - **논리 연산자 (&&, ||)**
        - && 논리 연산자는 첫번째 피연산자의 값이 true로 평가 되는 경우 두번째 피연산자의 값을 반환하고 false로 평가가 되면 첫번째 피 연산자의 값을 반환함
        - || 논리 연산자는 첫번째 피연산자의 값이 true로 평가 되는 경우 첫번째 피연산자 값을 반환하고, false로 평가되면 두번째 피연산자의 값을 반환한다
        
        ```jsx
        const a = null;
        const b = 'js';
        const c = 1;
        
        console.log(a && b) // null
        console.log(b || c)  // 'js'
        
        /**
        	* <&& 논리연산자 평가>
        	* 1. 첫번째 피연산자 a부터 평가.
        	* 2. 첫번째 피연산자 a는 불리언 값이 아니므로, 암시적 타입변환을 통해 불리언값으로 변환,
        	*    null은 falsy한 값이기 때문에 false로 변환됨
        	* 3. a의 평가 결과가 false기 때문에 단락 평가 방식에 따라 다음 피연자인 b는 평가 x 
        	* 4. 피연산자 a 반환
        	*
        	* <|| 논리연산자 평가>
        	* 1. 첫번째 피연산자 b부터 평가.
        	* 2. 첫번째 피연산자 b는 불리언 값이 아니므로 암시적 타입변환을 통해 불리언값으로 변환
        	*    'js'는 truthy한 값이기 때문에 true로 변환
        	* 3. b의 평가 결과가 true기 때문에 단락 평가 방식에 따라 다음 피연자인 c는 평가 x
        	* 4. 피연산자 b 반환
        	*/
        
        ```
        
    
    - **논리 연산자의 활용** 
    디폴트 값을 설정하거나 조건에 따라 함수 실행 할때 유용
        
        ```jsx
        function setDefault(a){
        	return a || 'default string'
        }
        ```
        
    

---

## 함수

함수는 객체의 특별한 형태이며, **문(statement)로 구성된 몸체를 가진 하나의 실행단위** 

```jsx
function sayHello() {
   return "Hello, ";
}
```

자바스크립트는 **일급 함수 (first-class function)**으로서 다른 함수의 매개변수나 반환값으로 사용할수 있음.

```jsx
function sayHello() {
   return "Hello, ";
}
function greeting(helloMessage, name) {
  console.log(helloMessage() + name);
}
// `sayHello`를 `greeting` 함수에 인자로 전달
greeting(sayHello, "JavaScript!");
```

<aside>
📌 다른 함수에 인자로 전달된 함수는 [콜백함수](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function), sayHello()는 콜백 함수임

</aside>

### 함수 정의 방법

- **함수 선언문**
함수의 이름, 매개변수, 몸체로 구성. **함수의 이름이 반드시 정의되어야함**
    
    ```jsx
    console.log(multiply(2,3)) // 6
    
    function multiply(a,b) {
    	return a * b
    }
    
    // 함수 선언문은 호이스팅(hoisting)으로 인해 함수가 선언된 위치에서 최상단으로 끌어올려짐
    ```
    
- **함수 표현식**
함수 선어문과 달리 **함수 이름이 선택 사항** 임. 변수에 함수를 직접 할당하는 방식  ****
    
    ```jsx
    const multiply = function (a,b) {
    	return a * b
    }
    
    console.log(multiply(2,3)) // 6
    ```
    
    위의 코드는 함수에 이름이 없고 변수에 이름을 할당한것임. 이름이 없는 함수를 **익명함수 (anonymous function) 표현식**이라고 부름. 익명함수의 호출은 함수를 할당한 변수를 사용하여 호출 
    
    ```jsx
    const multiply = function doSomething(a,b) {
    	return a * b
    }
    
    console.log(multiply(2,3)) // 6
    console.log(doSomething(2,3)) // ReferenceError
    ```
    
    위의 코드처럼 **함수의 이름이 작성되어있는것을 기명함수 표현식** 라고 부름. 외부에서 호출의 경우 반드시 함수를 할당한 변수를 사용해야함.
    
    ```jsx
    // 기명함수 표현식은 주로 재귀적으로 호출시 사용 
    const factorial = function doSomething(n) {
    	return n <= 1 ? 1 : n * doSomething(n-1);
    }
    console.log(factorial(5)) // 120
    
    // **함수 표현식은 호이스팅이 되지 않기 때문에** 
    // 변수를 함수에 할당하기전에 호출할수 없음 
    console.log(multiply(2,3)) // Uncaught TypeError
    
    const multiply = function doSomething(a,b) {
    	return a * b
    }
    ```
    

### 화살표 함수

화살표 함수는 항상 익명함수이며, 아래와 같은 문법적 특징이 있음

- function 키워드를 생략
    
    ```jsx
    const greeting  = () => {return 'hello'}
    ```
    
- 매개변수가 하나인 경우 괄호를 생략
    
    ```jsx
    const greeting  = name => {return `hello ${name}`}
    ```
    
- 함수 몸체에서 문이 하나인 경우 중괄호({})나 retrun 키워드를 생략 할수있음
    
    ```jsx
    const greeting  = name => {return `hello ${name}`}
    ```
    
### 화살표 함수의 특징

화살표 함수는 argument와 this 를 바인딩 하지 않기 때문에 기존 함수와 다르게 동작한다.

```jsx
const func = () => argument[0]
func(1) // Uncaught ReferenceError : argument is not defined
```

화살표 함수에서는 나머지 매개변수를 사용하여 arguments 객체를 대체할수있다.

```jsx
const func = (...args) => args[0]
func(1) // 1
```

**화살표 함수는 일반 함수와는 달리 고유한 this를 가지지 않음**. 화살표함수에서 this를 참조하면, 화살표 함수가 아닌 평범한 외부함수에서 this값을 가져옴 

```jsx
let user = {
  firstName: "보라",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // 보라
```

---

## this

자바스크립트에는 this라는 키워드를 사용할수있는데 this는 읽기 전용 값으로 런타임시 설정할수 없으며, **함수를 호출한 방법에 의해 값이 달라짐**.

**자바스크립트에서 this는 런타임에 따라서 결정**됨. 메서드가 어디서 정의되었는지 상관없이 **this는 ‘점 앞의' 객체가 무엇인가에 따라 ‘자유롭게' 결정됨** 

**전역 실행 컨텍스트**에서 this는 항상 **전역 객체를 참조**함

**전역 실행 컨텍스트**란? 자바스크립트 엔진이 코드를 실행할 때 처음으로 생성되는 컨텍스트, **자바스크립트 코드가 실행되는 최상위 환경**이라고 볼수있음  

<aside>
📌 실행되는 환경에 따라 달라지는데 **브라우저**에서는 **window**가 **node js**에서는 **global** 이 전역 객체임

</aside>

```jsx
// 브라우저 환경에서 console.log(this)를 하였을때 window 객체가 나오는것을 알수있다
console.log(this)

// VM1862:1 Window {window: Window, self: Window, document: document, name: '', location: Location, …}
```

### 일반 함수

함수 선언문 혹은 함수 표현식으로 정의한 함수를 호출할 경우 this 바인딩은??

```jsx
// 브라우저 환경에서 함수를 호출할 경우 this가 window 객체를 바인딩 함.
function func(){ console.log(this)}
// -> undefined
func()
// -> VM2156:1 Window {window: Window, self: Window, document: document, name: '', location: Location, …}
```

위의 코드와 같이 함수안에서 호출한 this역시 전역객체를 참조한다. 하지만 **함수에서 this는 전역객체를 참조할것이 아니라 undefined가 되어야함.** 왜냐하면 **함수의 컨텍스트가 어디에 속하는지 알수없기 때문에** 

이때문에 **es5에서는 ‘엄격 모드’**가 나왔으며 **엄격하게 오류를 검사하고 안전한 자바스크립트 작성**을 도와줌.

```jsx
function strictFunc(){ 
    'use strict';
    console.log(this)
}

strictFunc()
// -> undefined
```

### 생성자 함수

**생성자 함수**란? **객체를 만들때 사용하는 함수** 

생성자 함수와 일반함수의 기술적인 차이는 없으나 this 바인딩시 다르게 동작함. **new라는 키워드를 사용**

```jsx
function Vehicle(type) {
	this.type = type;
}
const car = new Vehicle('car')

// 아래와 같이 객체가 생성된것을 알수있다.
car
// -> Vehicle {type: 'car'}
```

**객체가 생성될때의 단계**

1. 객체를 생성하여 this에 바인딩
    
    → 빈 객체를 만들어 this에 할당합니다.
    
2. 프로퍼티생성 
    
    → 함수의 본문을 실행하여, this에 새로운 프로퍼티를 추가해 this를 수정합니다.
    
3. 객체반환 
    
    → 생성된 객체, this에 바인딩한 객체를 반환합니다. 반환값은 따로 명시하지않아도 this에 바인딩된 객체가 반환됨 
    

```jsx
function User(name) {
  // this = {};  (빈 객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가함
  this.name = name;
  this.isAdmin = false;

  // return this;  (this가 암시적으로 반환됨)
}
```

```jsx
let user = new User("보라");

alert(user.name); // 보라
alert(user.isAdmin); // false
```

`new User("보라")`이외에도 `new User("호진")`, `new User("지민")` 등을 이용하면 손쉽게 사용자 객체를 만들수 있음

### 메서드

객체의 프로퍼티인 함수를 일반함수와 구분하여 메서드라고 부르며, **메서드를 호출하면 this는 해당 메서드를 소유하는 객체로 바인딩** 됨, 

메서드 내부에서 this 키워드를 사용하면 객체에 접근할수있음. 이때 ‘점 앞'에 this는 객체를 나타냄.

```jsx
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // 'this'는 '현재 객체'를 나타냅니다.
    alert(this.name);
  }

};

user.sayHi(); // John
```

**메서드를 어떻게 호출했냐에 따라서 this 바인딩이 달라짐** 

**메서드를 의도한대로 사용하기 위해서는 반드시 해당 객체의 컨텍스트를 명확하게 지정하여 호출하여야함** 

---

## call(), apply(), bind()

**함수의 호출방법에 상관없이 this를 특정한 객체로 바인딩 할때 사용**함. 이러한 바인딩 방법을 명시적 바인딩이라고 부름

### call(), apply()

call()은 주어진 this값 및 각각 전달된 인수와 함께 호출한다. 첫번째 인자로 this를 바인딩할 객체를 지정함.

```jsx
const obj = {name: 'javascript'};

function greeting(){
		return `Hello ${this.name}`
}

console.log(greeting.call(obj)) // Hello javascript
```

call() 메서드를 통해 호출하는 함수로 인자를 전달할수 있음.

```jsx
const obj = {name: 'lee'};

function greeting(){
		return `Hello ${this.name} ${age} ${country}`
}

console.log(greeting.call(obj,20,'kr')) // Hello lee 20 kr
```

call()메서드는 첫번째 인자 이후의 인자들은 모두 호출하는 함수로 전달됨 

apply()는 call()과 동일하지만 호출하는 함수에 전달할 인자들을 배열의 형태로 전달함 

```jsx
const obj = {name: 'lee'};

function greeting(){
		return `Hello ${this.name} ${age} ${country}`
}

console.log(greeting.call(obj,[20,'kr'])) // Hello lee 20 kr
```

### bind()

bind()는 call()과 apply()와의 두가지 차이점을 가지고 있음 

- **this**의 바인딩을 **영구적으로 변경**한다. (생성자 함수 제외)
- **this**를 바인딩하여 함수를 호출하는것이 아니라 **새로운 함수를 반환**함.

bind()메소드는 this의 값을 고정시킬때 사용하면 좋음

```jsx
const obj1 = {name:'lee'};
const obj2 = {name:'han'};

function getUserInfo(age, country){
	return `${this.name}, ${age}, ${country}`
}

const bound = getUserInfo.bind(obj)
console.log(bound(20,'kr')) // lee, 20, kr
// this 값이 영구적으로 고정되었기 때문에 어디서 호출하든 obj1의 값이 호출됨 
console.log(bound.apply(obj2, [20,'kr'])) // lee, 20, kr
```

---

## 화살표 함수와 렉시컬 this

### 렉시컬 this

화살표 함수는 화살표함수를 둘러싸고있는 렉시컬 스코프에서 this의 값을 받아와 사용함. 이것이 렉시컬 this

```jsx
const obj = {
	lang: 'javascript'
	greeting: ()=> {
		return `hello ${this.lang}`
	}	
}
// 화살표 수의 렉시컬 this가 obj가 아닌 obj를 둘러싼 전역 컨텍스트에서 값을 받아오기 때문에 undefined
console.log(obj.greeting()) // 'hello undefined'
```

화살표 함수는 call, apply, bind를 이용해서 this를 변경할수 없다

```jsx
const obj = {
	lang: 'javascript'
}
const	greeting =()=> {
		return `hello ${this.lang}`
}	

console.log(greeting.call(obj)) // 'hello undefined'
```

화살표 함수는 정적인 렉시컬 this를 사용하기 때문에 기존의 동적인 this 바인딩의 혼잡함에서 벗어나 단순하게 사용 할 수 있음.

특히 setTimeout()과 같은 전역 객체의 함수를 메서드에서 호출할 경우 화살표 함수를 활용하면 간결하게 표현가능 

```jsx
// 일반함수 
const obj = {
	name: 'javascript',
	greeting() {
		setTimeout((function timer() {
			console.log(this.name)
		}).bind(this), 1000),
	}
}

// 화살표 함수 
const obj = {
	name: 'javascript',
	greeting() {
		setTimeout(()=> {
			console.log(this.name)
		},1000),
	}
}
```