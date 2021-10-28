---
layout: post
type: LOading
date: 2021-10-10 20:13
category: Typescript
title: enum 타입에 대해서
subtitle: 이넘...너 뭐하는 놈이냐..!
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Typescript, enum]
---



자바스크립트만 공부했다면 enum은 굉장히 생소하게 느낄수 있다.

enum은 특정 값들의 집합을 의미하는 자료형을 뜻한다.

이런 enum을 타입처럼 사용할수도 있고 값으로도 사용할수가 있다.

타입을 정의하는 위치에 놓이면 타입이 되고, 값을 정의하는 위치에 놓이면 값이 된다.

이넘의 첫번째 원소에 값을 할당하지 않으면 0이 할당 된다.

이넘의 각 원소에는 숫자 또는 문자열을 할당할 수 있다.

명시적으로 값을 입력하지 않으면 이전 원소에서 1만큼 증가한 값이 할당된다.

 

무슨 여기까지 읽으면 무슨소리인지 이해가 되지 않을것이다.

아래 예시를 보자.

```
export {};

enum Fruit {
	Apple,
	Banana = 5,
	Orange,
}

console.log(Fruit.Apple, Fruit.Banana, Fruit.Orange); // 0 5 6

console.log(Fruit.Banana); //5
console.log(Fruit['Banana']); //5
console.log(Fruit[5]); //Banana
```

enum의 위치는 const나 let, var와 동일한 자리이다.

뒤에 집합(그룹)의 이름은 지정해준뒤, {}를 열어 안에 내용을 적어 넣는다.

위 타입스크립트를 컴파일 하게 되면 아래와 같다.

```
var Fruit; //변수 선언

(funcion (fruit) {
	Fruit[(Fruit['Apple'] = 0)] = 'Apple';
	Fruit[(Fruit['Banana'] = 5)] = 'Banana';
	Fruit[(Fruit['Orange'] = 6)] = 'Orange'; //이름과 값이 양방향 맵핑이 됨
})(Fruit || (Fruit = {})); // 객체할당
console.log(Fruit.Apple, Fruit.Banana, Fruit.Orange);
```

이를 보면 먼저 Fruit를 선언한뒤 객체를 할당하고, 안에 값을 넣고 있다.

그리고 enum의 각 원소가 키값과 벨류값이 양방향으로 맵핑되는것을 확인할수 있다.

enum 아이템의 값은 숫자 뿐만 아니라 문자열을 입력할수도 있다.

```
export {};

enum Language {
	Korean = 'ko' ,
	English = 'en',
	Japanese = 'jp',
}
var Language; //변수 선언

(function (Language) {
	Language['Korean'] = 'ko';
	Language['English'] = 'en';
	Language['Japanese'] = 'jp';
})(Language || (Language = {}));
```

enum의 원소에 문자열을 할당하는 경우에는 단방향으로 맵핑된다.

숫자는 양방향 맵핑되지만 문자열의 경우 단방향 맵핑이 된다.

이를 이해했다면 아래와 같은 유틸리티 함수를 작성할 수 있다.

```
export {};

function getIsValidEnumValue(enumObject: any, value: number | string) {
	return Object.keys(enumObject)
		.filter(key => isNaN(Number(key)))
		.some(key => enumObject[key] === value);
}
```

이 함수는 어떤 enum 객체에 특정 value가 있는지 검사하는 함수이다.

먼저 enum 객체에서 모든 키를 뽑아낸 다음에 마지막에는 enum 객체 안에 입력된 value가 있는지 검사하고 있다. 중간에 filter 를 하는 이유는 양방향 맵핑 때문이다.

이 함수를 활용하여 다음과 같은 결과를 확인해 보자.

```
export {};

function getIsValidEnumValue(enumObject: any, value: number | string) {
	return Object.keys(enumObject)
		.filter(key => isNaN(Number(key)))
		.some(key => enumObject[key] === value);
}

enum Fruit {
	Apple,
	Banana,
	Orange,
}
enum Language {
	Korean = 'ko' ,
	English = 'en',
	Japanese = 'jp',
}

console.log('0 in Fruit : ', getIsValidEnumValue(Fruit, 0));
console.log('1 in Fruit : ', getIsValidEnumValue(Fruit, 1));
console.log('2 in Fruit : ', getIsValidEnumValue(Fruit, 2));
console.log('5 in Fruit : ', getIsValidEnumValue(Fruit, 5));
console.log('Orange in Fruit : ', getIsValidEnumValue(Fruit, 'Orange'));
console.log('ko in Language : ', getIsValidEnumValue(Language, 'ko'));
console.log('Korean in Language : ', getIsValidEnumValue(Language, 'Korean'));
결과

0 in Fruit :  true
1 in Fruit :  true
2 in Fruit :  true
5 in Fruit :  false
Orange in Fruit :  false
ko in Language :  true
Korean in Language :  false
```

Fruit 안에는 원소값이 할당되지 않았기 때문에 Apple은 0, Banana는 1, Orange는 2가 된다. 그래서 0,1,2 는 있지만 5는 없다.

Orange는 값이 아니고 이름이기 때문에 false이다.

Language도 마찬가지이다.

enum을 사용하면 컴팡리 후에도 enum 객체가 남아 있기 때문에 번들 파일의 크기가 불필요하게 커질 수 있다. enum 객체에 접근하지 않는다면 굳이 컴파일 후에 객체로 남겨 놓을 필요가 없다.

그럴때 const enum을 사용하여 결과에 enum의 객체를 남겨 놓지 않을 수 있다.

```
 export{};

const enum Fruit {
	Apple,
	Banana,
	Orange,
}
const fruit: Fruit = Fruit.Apple;

const enum Language {
	Korean = 'ko' ,
	English = 'en',
	Japanese = 'jp',
}
const lang : Language = Language.Korean;
```

이 코드를 컴파일 하면 다음과 같이 나온다.

```
"use strict";
exports.__esModule = true;
var fruit = 0 /* Apple */;
var lang = "ko" /* Korean */;
```

확인해 보면 enum객체가 안보이고 사용한 값만 노출되고 있는것을 확인할수 있다.

const enum을 사용하면 getIsValidEnumValue 유틸함수는 사용할수가 없다. Fruit라는 enum객체가 없기 때문이다.
