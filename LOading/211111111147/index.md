---
layout: post
type: LOading
date: 2021-11-11 11:47
category: Computer Science
title: 구조분해 할당
subtitle: 구조를 막 분해해서 할당하자~!
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [기술면접, ES6, 구조분해할당]
---

구조분해 할당은 Javascript ES6 문법중에 추가된 것중 하나이다.

ES5에서 ES6 로 넘어오면서 여러가지 편의 기능들이 추가되었다.

나머지 기능들은 다음에 다뤄보기로 하고 오늘은 구조분해 할당에 대해 정리해 볼까 한다.

# 구조분해 할당이란?

배열이나 객체에 저장된 데이터중 전체가 아닌 일부만 필요할때,

원하는 데이터만 분해해서 할당 하는것이 구조분해할당이다.

말 그대로 구조(배열,객체)를 분해해서 할당 시킨다.

<br>

<br>

## 객체의 구조분해 할당

사용법은 다음과 같다.

```jsx
const data = {Id:1, name:KimLink, age:21}

let {Id, name} = data
```

변수를 객체안에 넣어 선언해주고, 우측에 저장된 데이터를 객체로 할당해준다.

위 예제에서 기존데이터에서 Id, name, age가 들어있었는데,

그중에 Id값과 name값만 필요하다면 위와같이 구조분해 할당으로 정리 할수가 있다.

이것을 만약 ES5 문법에서 사용한다고 하면,

```jsx
const data = {Id:1, name:KimLink, age:21}

let Id = data.Id;
let name = data.name;
```

이렇게 된다.

지금은 두줄과 한줄의 차이 이지만,

만약 데이터가 10줄이라고 가정할때 ES5 문법에서는 10줄을 작성해야 하지만, ES6 문법에서는 한줄로 정리가 된다.

객체의 특성상 key값이 중요하기 때문에 할당받는 변수의 순서가 달라도 상관이 없다.

하지만 할당받을 변수의 이름을 다르게 하고 싶을때는 어떻게 해야 할까?

그때는 콜론을 이용하여 바꿀수 있다.

```jsx
const data = {Id:1, name:KimLink, age:21}

// { 객체 프로퍼티: 목표 변수 }
let {Id : userId, name : userName} = data
```

<br>

그다음 객체 구조분해 할당에서 알아두면 유용한것이 '...rest' 이다.

```jsx
let Menu = {
  name: "Americano",
	cost: 4500,
  size: 330
};

let {name, ...rest} = Menu;

console.log(rest.cost); // 4500
console.log(rest.size);  // 330
```

보는 바와 같이 구조분해 할당후 나머지 데이터만 따로 저장해 놓고 싶을때 '...rest'를 사용할수 있다.

name에는 "Americano"가 들어가게 되지만,

rest에는 {cost: 4500, size: 330}가 할당 되게 된다.

name의 형태는 string으로, rest는 object가 된다.

<br>

<br>

# 배열의 구조분해 할당

사용법은 다음과 같다.

```jsx
let arr = ["Link", "Kim"]

let [firstName, surname] = arr;

console.log(firstName); // Link
console.log(surname);  // Kim
```

변수를 배열안에 넣어 선언해주고, 우측에 저장된 데이터를 배열로 할당해준다.

배열의 구조분해 할당에서 중요한점은 순서이다. 배열은 Index를 가지고 있기 때문이다.

<br>

 배열 구조분해 할당에서도 '...rest'를 사용할수 있다.

```jsx
let metals = ["Fe", "Al", "Cu", "Si"];
let [metal1, metal2, ...rest] = metals

console.log(metal1); // Fe
console.log(metal2);  // Al

console.log(rest[0]); // Cu
console.log(rest[1]);  // Si
console.log(rest.length);  // 2
```

이처럼, 순서대로 원하는 값을 할당하고 나서 나머지 데이터도 어딘가 담아 두고 싶을때 '...rest'를 사용하여 저장할수 있다.

이때는 rest라는 변수에 나머지 데이터들이 배열로 들어가 있게 된다.

<br>

<br>

# 정리

- 객체는 객체를 열어 Key값을 넣어 할당해줄것.
- 배열을 배열을 넣어 순서대로 할당해 줄것.
- 객체는 객체를 열고, 배열을 배열을 열어 분해 할당 하지만, 객체와 배열이 생성되는것이 아니고 각각의 선언된 변수 명에 할당된다.
- 객체, 배열 모두 rest pattern을 사용하여 나머지 데이터들을 따로 저장할수가 있다.

<br>

<br>

### 참고링크 :

https://ko.javascript.info/destructuring-assignment

https://tech.toktokhan.dev/2021/08/22/es6/
