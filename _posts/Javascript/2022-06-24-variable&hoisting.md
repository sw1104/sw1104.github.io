---
title: "변수와 호이스팅, TDZ, scope"
excerpt: "Javascript Variable"

categories:
  - javascript
tags:
  - [javascript, variable, hoisting, TDZ, scope]

permalink: /javascript/variable-hoisting-tdz-scope

toc: true
toc_sticky: true

date: 2022-06-24
last_modified_at: 2022-06-24
---

## 변수 선언
변수 선언은 `var`, `let`, `const` 키워드를 이용하여 할 수 있다.
ES5까지는 `var`가 쓰였고 몇가지 문제점이 있어 ES6에서 `let`, `const`가 추가되었다.

---
### `var`, `let`, `const`의 차이는 다음과 같다.

# 변수 선언방식
### var는 중복 선언이 가능하다.
```javascript
var name = "Sang";
console.log(name); // Sang
var name = "Woo";
console.log(name); // Woo 
```
이는 변수를 유연하게 사용할 수 있다는 장점이 될 수 있지만, 몇백줄 몇천줄의 코드에서 기존에 선언한 변수에 재할당하게 되는 실수가 발생할 수 있는 위험이 있고 찾기도 어렵다.

### let은 중복 선언은 불가능하고 재할당은 가능하다.
```javascript
let name = "Sang";
console.log(name); // Sang
let name = "Woo";
console.log(name); // Uncaught SyntaxError: Identifier 'name' has already been declared 

name = "Sangwoo";
console.log(name); // Sangwoo
```

`let`으로 이미 선언한 변수 `name`에 다른 값을 넣었을 때 이미 `name`이라는 변수가 선언 되었다는 에러가 출력된다. 
그러나 `name = "Sangwoo";` 처럼 값을 재할당 하는것은 가능하다.

### const는 중복 선언, 재할당 모두 불가능하다.

```javascript
const name = "Sang";
console.log(name); // Sang

const name = "Woo";
console.log(name); // Uncaught SyntaxError: Identifier 'name' has already been declared

name = "Sangwoo";
console.log(name); // Uncaught TypeError: Assignment to constant variable.
```
`const`로 선언한 변수 `name`에 값을 "Woo"로 중복선언 했을 경우 `let`과 같은 에러 메시지가 출력되었다. 
그리고 `name`에 값을 재할당 했을 때 `let`과 달리 에러 메시지가 출력되었다.

***
# 호이스팅
먼저 호이스팅이란 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미한다. 쉽게 변수와 함수의 선언을 해당 스코프의 최상단으로 끌어올려서 선언하는 것이다. 
실제로 코드가 바뀌는 것은 아니며, 자바스크립트의 파서(Parser)가 함수 실행 전에 내부적으로 선언문을 끌어올려서 처리한다.
호이스팅은 모든 변수와 함수선언식에 영향을 미친다. 그러나 `var`와 달리 `let`과 `const`는 `일시적 사각지대(TDZ : Temporal Dead Zone)`에 의해 호이스팅 되지 않은것처럼 보인다.

### 먼저 호이스팅을 이해하기 위해선 변수가 생성되는 과정을 알아야한다.

### 변수 선언 3단계
자바스크립트에서 변수는 `선언 단계` -> `초기화 단계` -> `할당 단계`로 생성이 된다.

   1. 선언 단계 (Declaration phase)
   선언 단계에서는 변수를 실행 컨텍스트의 변수 객체에 등록한다. 이 변수 객체는 스코프가 참조하는 대상이 된다.
   2. 초기화 단계 (Initialization phase)
   초기화 단계에서는 변수 객체에 등록된 변수를 위해 메모리에 공간을 확보하며, 변수는 `undefined`로 초기화된다.
   3. 할당 단계 (Assignment phase)
   할당 단계는 초기화 단계에서 `undefined`로 초기화된 변수에 값을 할당한다.

## 변수에서 호이스팅

### var


```javascript
console.log(name); // undefined
var name = "Sang";
```
`var`는 호이스팅 되기 때문에 첫줄에서 에러가 나타나지 않고 `undefined`가 나타났다. 이는 `var` 키워드를 이용한 변수는 `선언 단계`와 `초기화 단계`가 동시에 일어나기 때문이다. 위 코드가 실제 작동되는 것을 풀어서 작성하면 다음과 같이 `선언` + `초기화` 부분이 호이스팅 된 것을 알 수 있다.
```javascript
var name;
console.log(name); // undefined
name = "Sang";
```

### let
```javascript
console.log(name); // ReferenceError: name is not defined
let name = "Sang";
```
`let`으로 변수를 선언하니 에러가 발생했다. 위에만 보고 오해할 수 있는게 `let`은 호이스팅 되지 않는다고 생각할 수 있다. 그러나 `let`또한 호이스팅 된다.

```javascript
var name = "Sang"; // 전역변수
{console.log(name); // ReferenceError: Cannot access 'name' before initialization
 let name = "Woo";} // 지역변수
```
만약 `let`이 호이스팅 되지 않았다면 위에서 출력된 `name`은 전역변수인 `"Sang"`을 불러와야하지만 실제론 { } 내부의 `let`으로 선언된 `name`변수가 호이스팅 되었기 때문에 에러가 나타났다.
이렇게 나타나는 이유는 `let`은 `var`와 달리 변수가 `선언 단계`, `초기화 단계`, `할당 단계`가 독립적으로 일어나기 때문이다.
그렇기 때문에 에러 메시지도 초기화 전에는 이름에 접근할 수 없다고 나타나는 것이다.

### const
`const`또한 `let`과 같다.
```javascript
const name; // SyntaxError: Missing initializer in const declaration

const name = "Sw";
console.log(name); // Sw
```
다만 `const`에서는 `선언 단계 , 초기화 단계, 할당 단계`가 동시에 일어나게 된다. 그래서 할당할 값이 없이 선언만 하게 되면 에러가 발생하게 된다.


## 함수에서 호이스팅
### 함수선언문
```javascript
myName(); // "Sang"

function myName(){
	console.log("Sang");
}
```
함수 선언문에서는 정상적으로 호이스팅되어 잘 작동한다.
### 함수표현식
```javascript
// var를 이용한 함수표현식
myName(); // TypeError: myName is not a function

var myName = function(){
	console.log("Sang");
}

// let을 이용한 함수표현식
myName(); // ReferenceError: Cannot access 'myName' before initialization

let myName = function(){
  console.log("Woo");
}
  
```
`var`를 이용한 함수표현식에서는 TypeError가 발생했다. `var`를 이용했기 때문에 정상적으로 선언과 초기화가 됐지만 함수가 할당이 되지 않았기 때문에 함수가 아니라는 에러메시지가 나타난 것이다.
`let`을 이용한 함수표현식에서는 ReferenceError가 발생했다. `let`을 이용했기 때문에 선언만 될 뿐 초기화가 되지 않아서 할당값에 접근조차 하지 못하는 것이다.


---


# TDZ(Temporal Dead Zone)
`일시적 사각지대(TDZ : Temporal Dead Zone)`는 스코프의 시작 지점부터 초기화 시작 지점까지의 구간을 말한다. `let`과 `const`가 호이스팅이 됨에도 에러가 발생하는것도 이것에 영향을 받기 때문이다.
```javascript
function myName() {
  console.log(name); // TDZ
  let name = "Sang";
}
myName(); // ReferenceError: Cannot access 'name' before initialization
```
`myName`함수 스코프의 시작부터 `let`이 호이스팅이 되고 초기화되기 전까지인 `console.log(name);` 부분이 TDZ영역이 된다. 

# 스코프

스코프는 식별자 접근 규칙에 따른 유효범위를 뜻한다. 범위는 `중괄호{ }`, 또는 `함수`에 의해 나눠진다.

### 함수 레벨 스코프(function level scope)
```javascript
function myName(){
	if(true){
    	var name = "Sang";
        console.log(name); // Sang
    }
  console.log(name); // Sang
}

console.log(name); // ReferenceError: name is not defined
```
`vat`키워드는 함수 레벨 스코프를 갖는다. 함수내에서 선언된 변수는 함수 안에서 자유롭게 사용할 수 있는 지역변수이며, 함수 외부에서는 이용할 수 없다.  함수 외부에서 선언된 변수는 전역변수라 한다.

### 블록 레벨 스코프(block level scope)
```javascript
function myName(){
	if(true){
      let name1 = "Sang";
      console.log(name1); // Sang
      const name2 = "Woo";
      console.log(name2); // Woo
    }
  console.log(name1); // ReferenceError: name1 is not defined 
  console.log(name2); // ReferenceError: name2 is not defined 
 
}
console.log(name1); // ReferenceError: name1 is not defined 
console.log(name2); //ReferenceError: name2 is not defined 

```
`let`, `const`키워드는 블록 내에서만 사용할 수 있다.

## 정리
- ES5로 코드를 작성할게 아니라면 `var`는 자제하는것이 좋다.
- `const`주로 이용하며 재할당이 필요한 경우에만 `let`을 이용하자.
- 가급적 호이스팅이 일어나지 않도록 코드를 짜는것이 좋다.
- 변수의 스코프는 최대한 좁게 만드는것이 좋다.
