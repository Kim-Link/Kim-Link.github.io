---
layout: post
type: LOading
date: 2021-10-12 13:12
category: Typescript
title: 함수 타입(2)
subtitle: 함수에서는 타입을 어떻게 정의할까?
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Typescript, 함수 타입]
---



```tsx
export{};

function getParam(this: string, index: number) : string {
    const params = this.split(',');
    if(index < 0 || params.length <= index) {
        return '';
    }
    return this.split(',')[index];
}
```

타입스크립트에서 this의 타입도 정의를 할수가 있다. 함수 제일앞에 this : string 으로 표시를 해주면 this를 string으로 인식을 한다. 그래서 함수의 매개변수에 대한 타입은 두번째부터 시작하게 된다.

자바스크립트에 내장된 타빙에 기능을 주입하고 싶을때는 prototype을 사용하여 주입을 할수가 있다.

prototype을 주입할때 이 prototype에 대한 타입이 정해져 있지 않기 때문에 에러가 발생할수 있다.

이럴 경우, interface를 이용하는 방법이 있다.

```tsx
function getParam(this: string, index: number) : string {
    const params = this.split(',');
    if(index < 0 || params.length <= index) {
        return '';
    }
    return this.split(',')[index];
}

interface String{
    getParam(this: string, index: number): string;
}

String.prototype.getParam = getParam;
console.log('asdf, 1234, ok'.getParam(1));
```

- 함수 오버로드??

함수를 정의한 코드 위에 같은 이름으로 코드의 타입을 정의한것.

자바스크립트로 컴파일 했을때 오버로드된 정보들은 제거가 됨.

```tsx
export {};

//함수 오버로드
function add(x: number, y: number) : number;
function add(x: string, y: string) : string;
function add(x: number | string, y : number|string) : number | string {
    if(typeof x === 'number' && typeof y === 'number') {
        return x + y;
    } else {
        const result = Number(x) + Number(y);
        return result.toString();
    }
}

const v1 : number = add(1,2);
console.log(add(1, '2')); // 에러발생 (에러가 발생해야됨)
```

- named parameters 방식

전체를 객체로 감싸주고 그 뒤에 타입을 정의해 주는것.

함수를 사용할때는 이름을 적엇 사용할수가 잇다.

```tsx
export{};
function getText({
    name,
    age  = 15,
    language,
} : {
    name : string;
    age? : number;
    language? : string;
}) : string {
    const nameText = name.substr(0,10);
    const ageText = age >= 35 ? 'senior' : 'junior';
    return `name : ${nameText}, age : ${ageText}, laguage : ${language}`;
}
```

named parameter를 다른곳에서도 사용하고 싶다면 interface에 정의를 해놓고 인터페이스를 타입에 넣어주면 된다.

```tsx
export{};

interface Param {
    name : string;
    age? : number;
    language? : string;
}

function getText({ name, age = 15, language} : Param) : string {
    const nameText = name.substr(0, 10);
    const ageText = age >= 35? 'senior' : 'junior' ;
    return `name : ${nameText}, age : ${ageText}, laguage : ${language}`;
}
```

변수가 많아지면 named parameter로 사용하는것이 가독성에 좋다.

- 리팩토링 할때

named parameter로 처음부터 작성하지 않았다면 변환할때 조금 번거롭다.

다행히 typescript에서는 리팩토링 기능을 제공하고 있다.

이름을 클릭하면 손쉽게 변경이 가능하다.
