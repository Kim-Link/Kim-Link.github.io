---
layout: post
type: re-born
date: 2021-09-26 21:20
category: Javascript
title: 일급 객체, 함수 표현식, 함수 선언식, 스코프, 클로저, 원시 자료형, 참조 자료형
subtitle: 벌써 뭔가 머리가 아파온다
writer: 000000
post-header: true
header-img : img/about.png
hash-tag: [Javascript, 일급 객체, 함수 표현식, 함수 선언식, 스코프, 클로저, 원시 자료형, 참조 자료형]
---
>


---

### 일급 객체

자바스크립트에서는 특별한 대우를 받는 것들을 ***일급 객체*** 라고 부르며, 그 중 하나가 ***함수***이다.

자바스크립트에서 함수는

- 변수에 할당할 수 있다.
- 다른 함수의 인자로 전달될 수 있다.
- 다른 함수의 결과로 리턴될 수 있다.

이렇게 특별하게 취급된다.
이는 함수를 데이터처럼 다룰 수 있다는 것을 의미하고, 변수에 저장하여 배열의 요소나 객체의 속성값으로 저장하는 것도 가능하다.



### 함수 선언식, 함수 표현식

> #### 함수 선언식
>
> 일반적인 프로그래밍 언어에서의 함수 선언과 비슷한 형식이다
>
> ```js
> function example() {
>   return 'this is test function'
> }
> ```
>
> 

> #### 함수 표현식
>
> 유연한 자바스크립트 언어의 특징을 활용한 선언 방식이다
>
> ```js
> let example = function() {
>   return 'this is test function'
> }
> ```
>
> 

#### 왜 선언식과 표현식을 구분하여 사용하는가

|    종류     | 호이스팅 |
| :---------: | :------: |
| 함수 선언식 |    O     |
| 함수 표현식 |    X     |

함수 선언식은 코드를 구현한 위치에 상관하지 않고 자바스크립트의 특징인 ***호이스팅***에 따라
브라우저가 자바스크립트를 해석할 때 위치가 끌어올려진다.



> #### 호이스팅 예제
>
> 아래와 같이 코드를 작성하게 되면
>
> ```js
> logMessage();
> sumNumbers();
> 
> function logMessage() {
>      return 'worked';
>      }
> 
> var sumNumbers = function () {
>      return 10 + 20;
>      }
> ```
>
> 
>
> 컴퓨터는 아래처럼 인식하게 된다.
>
> ```js
> function logMessage() {
>       return 'worked';
>       }
> 
> var sumNumbers;
> 
> logMessage(); // 'worked'
> sumNumbers(); // Uncaught TypeError: sumNumbers is not a function
> 
> sumNumbers = function () {
>       return 10 + 20;
>       }
> ```
>
> 
>
> 함수 표현식으로 작성한 `sumNumbers`에서 `var`도 호이스팅이 적용되어 위치가 상단으로 끌어올려진 것을 볼 수 있다.
> 하지만 선언만 끌어올려지고, 할당은 호출 된 이후에 할당되므로, `sumNumbers`는 함수로 인식하지 않고 변수로 인식한다.
>
> 호이스팅에 대해 제대로 이해하지 못한 상태일지라도, 함수와 변수를 가급적 코드 상단부에서 선언하는 습관을 들이면 호이스팅으로 인한 스코프 오류는 사전에 방지할 수 있다.





### 스코프

사전적 의미인 '범위'와 유사하다. 다만 조금 더 좁은 의미로 ***변수 접근 규칙에 따른 유효범위***로 사용된다.

- 변수는 어떠한 환경 내에서만 사용 가능하며, 프로그래밍 언어는 각각의 변수 접근 규칙을 갖고 있다.
- 이 변수와 그 값이 유효한 지 판단하는 범위를 '스코프'라고 부른다.
- 자바스크립트는 기본적으로 함수가 선언됨과 동시에 자신만의 스코프를 갖는다.

> ```js
> let say = "hello";
> 
> function greeting() {
>   let firstName = 'Vanessa';
>   return say + ' ' + firstName;
> }
> 
> greeting(); // 'Hello Vanessa'
> firstName(); // ReferenceError
> ```
>
> 위 코드에서 `greeting`이라는 함수는 **Local Scope**로 불리고, 코드 전체는 **Global Scope**로 불린다.
>
> Global Scope에서 선언된 변수는 Local에서 접근이 가능하지만
> Local Scope 안쪽에서 선언된 변수는 Global에서 사용할 수 없다.
> 이 때문에 `greeting`이라는 함수는 정상적인 결과를 돌려주지만,
> `firstName`이라는 변수를 Global에서 호출했을 때는 찾을 수 없다는 오류를 반환한다.



#### 스코프 규칙

- Global에서 선언된 변수는 Local에서 접근이 가능하나, Local에서 선언된 변수는 Global에서 접근이 불가하다.

- Scope 는 중첩이 가능하다 (함수 안에 함수를 넣을 수 있다)
- Global Scope는 최상단의 Scope로, 어디서든 접근이 가능하다.
- 지역 변수는 함수 내에서 전역 변수보다 더 높은 우선순위를 가진다.
- Local 함수보다 더 범위가 작은 Block Scope도 있다. ***중괄호로 시작하고 끝나는 단위***로 이해하면 편하다.







### 클로저

방금 배운 스코프에서 일부 일정한 패턴을 갖는 것을 ***클로저***라고 부른다.

- 정의 : <u>**내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것**</u>을 뜻한다.
- 조금 더 자세히 설명하자면,
  스코프 규칙에 *Global에서 선언된 변수는 Local에서 접근이 가능하나, Local에서 선언된 변수는 Global에서 접근이 불가*하다는 규칙이 있는데, ***외부함수의 실행이 끝나서 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근할 수 있다.*** 이러한 매커니즘을 클로저라고 부른다.

> #### 클로저 예제
>
> ```js
> function outer(){
>     var person = 'Vanessa';  
>     function inner(){ 
>         let comment = 'Pretty'
>         return person + 'is' + comment
>     }
>     inner();
> }
> outer();
> ```
>
> 위 예제 코드를 실행하게 되면 클로저 패턴이 적용되어 `Vanessa is Pretty`가 결과로 리턴된다.





### 원시 자료형

고정된 저장 공간을 차지하는 데이터를 모두 **원시 타입(Primitive type)** 데이터라고 한다.
원시 타입 데이터는 **객체가 아니면서 method를 가지지 않는** 특징을 가지고 있다.

| string | number | bigint | boolean | undefined | symbol | (null) |
| :----: | :----: | :----: | :-----: | :-------: | :----: | :----: |

> #### 예시
>
> ```js
> const num = 123;
> const arr = [1, 2, 3, 4, 5];
> let word = 'hello world';
> ```
>
> 위 예시를 보면, 데이터의 크기와는 관계 없이 ***하나의 변수에는 하나의 데이터만***을 담을 수 있다.
> 원시 자료형은 값 자체에 대한 변경이 불가능(immutable)하지만, 변수에 다른 데이터를 할당할 수는 있다.



### 참조 자료형

원시 자료형이 아닌 모든 것들은 참조 자료형이다. (배열, 객체, 함수 등)
원시 자료형과 다르게 참조 자료형은 ***하나의 변수에 여러 데이터를 담을 수 있다***



(원리)
참조 자료형의 데이터는 **heap**이라고 불리는 별도의 데이터 보관함에 저장되고, 변수에는 데이터가 저장된 메모리 상의 주소가 저장된다.
원시 자료형과는 다르게 heap 안에 저장된 데이터는 원하는 대로 데이터 사이즈를 조정할 수 있다.

