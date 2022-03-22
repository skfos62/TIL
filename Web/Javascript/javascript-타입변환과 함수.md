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

---