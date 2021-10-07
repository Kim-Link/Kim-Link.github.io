---
layout: post
type: LOading
date: 2021-10-07 12:35
category: Typescript
title: Typescript의 기본타입(1)
subtitle: 타입스크립트의 기본타입 첫번째. 기본이 제일 중요한건 알지?
writer: 000000
post-header: true
header-img: img/headerImg.png
hash-tag: [Typescript, 기본타입]


---



타입스크립트가 타입스크립트인이유는

스크립트에 타입을 지정 하기 때문이다.

각 변수, 배열, 함수 등에 타입을 지정할수 있다.

이때 타입이 다르면 에러가 발생하고, vscode에서 에러를 바로 확인 할수가 있다.

에러에 대한 확인은 해당 블로그에서는 에러발생이라고 표시했지만 실제 vscode안에서는 빨간색 밑줄이 쳐진다.

 

#### [ 코드 형태 ]

```js
export{} ;

const size: number = 123;
const isBig : boolean = size >= 100;
const msg : string = isBig ? '크다' : '작다';

const values : number[] = [1,2,3];
const values : Array<number> = [1,2,3];
values.push('a'); //에러발생

const data : [string, number] = [msg, size];
data[0].substr(1);
data[1].substr(1); //에러발생

console.log('typeof 123 => ', typeof 123); // number
console.log('typeof "abc" => ', typeof 'abc'); // string
console.log('typeof [1,2,3] => ', typeof [1,2,3]); //object
```

Q. export{}; 는 왜 쓰나요??

⇒ 이름 충돌을 없애기 위해 사용한다. export{}; 를 쓰게 되면 이 자체를 모듈이라고 인식을 하게 되는데,

이 자체를 모듈로 인식하게 되면 이 코드 파일에서 변수의 스코프를 이 파일 안으로 제한할수 있다.

export{}; 설정을 안해주면 다른 ts파일의 함수이름이 동일할때 충돌이 발생한다.

 

Q. 타입 설정은 어떻게 이루어 지나요?

⇒ 보통 변수나 함수 뒤에 " : " 을 붙여서 타입을 결정할수 있다.

 

#### [ number, boolean, string 타입 ]

```js
const size: number = 123;
const isBig : boolean = size >= 100;
const msg : string = isBig ? '크다' : '작다';
```

→ 변수 선언뒤 " : " 을 붙이고 이 뒤에 type의 종류를 적게 된다.

 

#### [ 배열도 타입을 정할 수 있다. ]

```js
const values : number[] = [1,2,3];
const values : Array<number> = [1,2,3];
values.push('a'); //에러발생
```

→ 배열의 타입을 정하는 방법은 위에 보는것 처럼 두가지가 있다. 위 코드에서는 배열 안 요소들을 넘버로 정의한다.

→ 위 코드에서 에러가 발생한 이유는 배열의 타입을 number로 지정을 했는데 push를 통해 string을 넣으려고 하니 에러가 발생하게 된다.

 

 

[ 인덱스 별로 타입을 정할 수 있다. ] 

```js
const data : [string, number] = [msg, size];
data[0].substr(1);
data[1].substr(1); //에러발생
```

→ 각 인덱스 별로 타입을 정의 할수가 있다.