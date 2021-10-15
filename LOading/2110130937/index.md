---
layout: post
type: LOading
date: 2021-10-13 09:37
category: Typescript
title: 타입 호환성
subtitle: 타입과 타입이 충돌할때...
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Typescript]
---



타입 호환성은 어떤 타입을 다른 타입으로 취급해도 되는지 판단하는것이다.

정적 타입 언어의 가장 중요한 역활은 타입 호환성을 통해 컴파일 타임에 호한되지 않는 타입을 찾아내는 것이다.

어떤 변수가 다른 변수에 할당 가능하기 위해서는 해당변수의 타입이 다른쪽 변수의 타입에 할당 가능해야 한다.

할당 가능을 판단할 때는 타입이 가질수 있는 값의 집합을 생각하면 이해하기 쉽다.

```tsx
export {};

function func1(a : number, b : number | string) {
    const v1 : number | string = a;
    const v2 : number = b; //에러 발생
}
function func2(a: 1| 2) {
    const v1 : 1| 3 = a; //에러 발생
    const v2 : 1| 2| 3 =a; 
}
```

위 코드에서 에러가 발생하는 이유는 할당하는 부분에서 할당 가능하지 않기 때문에 에러가 나고 있다.

func1에서 에러가 발생한 이유는

숫자와 문자로 이루어진 집합이 숫자로만 이루어진 집합보다 더 크기 때문에 더 큰 값의 집합을 더 작은 값의 집합으로 넣을수가 없기 때문에 에러가 나는 것이다.

v2의 타입은 number이고 변수 b의 타입은 number 또는 string이다. 즉 v2에 변수 b가 들어올때 number로만 들어오지 않고 string으로 들어올수도 있기 때문에 에러가 발생한다.

반대로 v1의 타입은 number 또는 string 둘다 수 있는데 a는 number만 들어오기 때문에 에러가 발생하지 않는 것이다.

func2에서도 마찬가지 이다.

a는 1 또는 2가 올수 있고, v1에서는 1또는 3이 올수 있다. v1에 a를 할당 한다고 할때, a에 2가 들어오게 되면 v1에서는 할당 받을수 없기 때문에 에러가 발생한 것이다.

```tsx
export{};

interface Person {
    name : string;
    age : number;
}

interface Product {
    name : string;
    age : number;
}
const person : Person = { name: 'mike', age : 23};
const product : Product = person;
```

타입스크립트는 값 자체의 타입 보다는 값이 가진 내부 구조에 기반해서 타입 호환성을 검사한다. 이를 structural typing 이라 한다.

위 코드에서 Person 과 Product는 서로 타입 이름은 다르지만 내부 구조가 동일하다. 모든 속성과 그 타입이 같다.

이렇게 이름만 다르고 내부 구조가 같을 경우 서로 할당이 가능하다.

이처럼 structural typing을 사용하는 언어의 경우 내부 구조만 같아도 할당이 가능하다.

> 인터페이스 A 가 인터페이스 B로 할당 가능하기 위한 조건

1. B에 있는 모든 필수 속성의 이름이 A에도 존재해야 한다.
2. 같은 속성 이름에 대해, A의 속성이 B의 속성에 할당 가능해야 한다.

> 

```tsx
export{};

interface Person {
    name : string;
}
interface Product {
    name : string;
    age : number;
}
const obj = { name : 'mike' , age : '23', city : 'abc'};
let person: Person = obj;
let product: Product = obj; //에러 발생
//product = person;
//person = product;
```

위 코드에서 obj라는 객체를 만들었다.

이 obj에는 name이 있고 age가 있다. 그리고 다른 속성인 city가 있다.

이 obj 객체는 Person으로 할당 가능하다. name의 조건을 만족하기 때문이다. 뒤에는 어떤 값이 와도 상관이 없다.

반면 obj는 product로 할당 가능하지 않다. age가 숫자가 아니기 때문이다.

이렇게 person은 product보다 더 많은 값을 받아들일 수 있다. 값의 집합이 더 크기 때문이다.

person의 집합이 더 크기 때문에 더 작은 집합으로 넣을수가 없다.

반대로 product는 person으로 할당이 가능하다.
