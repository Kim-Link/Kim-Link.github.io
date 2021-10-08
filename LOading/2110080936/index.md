---
layout: post
type: LOading
date: 2021-10-08 09:36
category: Typescript
title: Typescript의 기본타입(2)
subtitle: 기본타입 두번째. 기본이 제일 중요한건 알지?
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Typescript, 기본타입]


---



기본타입 두번째.

### undefined, null

---

```tsx
export {};

let v1 : undefined = undefined ;
let v2 : null = null ;
v1 = 123; //에러발생

let v3 : number | undefined = undefined;
v3 = 123;

console.log('typeof undefined =>', typeof undefined); //undefined
console.log('typeof null =>', typeof null); // object
```

v1 은 undefined 만 가능하기때문에 에러가 난다.

v3의 경우 number 혹은 undefined가 가능하기 때문에 에러가 나지 않는다.

jvs에서 null은 object로 표현된다.



### literal도 타입으로 정의가 된다??

---

타입스크립트에서 재밌는 점은 숫자와 문자열의 리터럴도 타입으로 정의 할수가 있다.

```tsx
export {};

let v1 :10 | 20 | 30;
v1 = 15; //에러발생
v1 = 10;

let v2 : '경찰관' | '군인';
v2 = '소방관' //에러발생
v2 = '경찰관'
```



### any 타입

---

any 타입은 모든 타입이 가능한 형태이다.

```tsx
export {};

let value : any;
value = 123;
value = '456';
value = () => {};
```

any의 경우 기존에 js에서 작성된 코드를 typescript로 포팅할때 유용하게 사용 가능하다.

js에서 포팅된 모든 코드를 한번에 타입을 지정하기 어렵기 때문에 js에서 타입에러가 나는 부분만 any로 정의한다.

하지만 any를 남발하면 typescript를 쓰는 의미가 없다.



### void 타입

---

void 타입 : 아무값도 반환하지 않고 종료되는 함수의 반환 타입

never 타입 : 항상 예외가 발생해서 비정상적으로 종료 되거나 무한루프 때문에 종료 되지 않는 함수의 반환 타입(거의 사용할 일이 없음)

```tsx
export {};

function f1(): void {
	console.log('hello');
}

function f2(): never {
	throw new Error('some error');
}
function f3(): never {
	while (true) {
		//...
	}
}
```



### object 타입

---

객체도 타입이 될수가 있다.

```tsx
export {};

let v: object;
v = { name : 'abc' }
console.log(v.prop1); //에러 발생
```



### union, intersection  타입

---

union, intersection  타입을 이용해서 여러타입의 교집합과 합집합을 표현할수가 있다.

```tsx
export {};

let v1: ( 1 | 3 | 5 & 3 | 5 | 7);
v1 = 3;
v1 = 1; //에러 발생
```



### type 키워드

---

type 키워드를 이용해서 타입에 별칭을 줄수가 있다.

const, let, var 가 일반 변수를 선언한다고 하면

type은 타입 변수 선언한다고 생각하면 된다.

```tsx
export {};

type Width = number | string;
let width : Width;
width = 100;
width = '100px';
```

자주 쓰는 타입을 유니온으로 묶어서 타입 변수를 할당해서 사용하면 유용하다.