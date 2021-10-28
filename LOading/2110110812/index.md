---
layout: post
type: LOading
date: 2021-10-11 08:12
category: Typescript
title: 함수 타입(1)
subtitle: 함수에서는 타입을 어떻게 정의할까?
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Typescript, 함수타입]
---



함수의 타입은 다음과 같이 정의 할수 있다.

```jsx
export{};

function getText(name : string, age : number) : string {
    const nameText = name.substr(0, 10);
    const ageText = age >= 35 ? 'senior' : 'junior';
    return `name: ${nameText}, age: ${ageText}`;
}

const v1: string = getText('mike', 23);
const v2: string = getText('mike', '23'); //에러 발생
const v3: number = getText('mike', 23);  // 에러 발생
```

매개변수 오른쪽에 콜론을 입력하고, 타입을 정의한다. ⇒ (name : string, age : number)

반환 타입은 매개변수 괄호를 닫고 나서  콜론을 입력하고 타입을 정의하면 된다. ⇒ : string

v2의 경우 age가 숫자로 타입이 정의되었는데 문자로 입력했기 때문에 에러가 발생했다.

v3의 경우 함수의 반환 타입이 string으로 타입을 정의 되었는데 number로 v3의 타입을 number로 정의 했기 때문에 에러가 발생한다.

함수를 저장할 변수의 타입은 화살표 기호를 이용할수 있다.

```tsx
export{};

const getText: (name: string, age: number) => string = function(name, age){
    return '';
}
```

함수를 구현하는 코드에서는 따로 타입을 정의하지 않아도 된다.

(name: string, age: number) => string

여기 적힌 타입 정보를 이용하고 있기 때문이다.

매개변수 이름 오른쪽에 물음표 기호를 입력하면 선택 매개변수가 된다.(optional parameters)

```tsx
export{};

function getText(name: string, age: number, language?: string) : string {
    const nameText = name.substr(0,10);
    const ageText = age >= 35 ? 'senior' : 'junior';
    const languageText = language ? language.substr(0, 10) : '';
    return `name: ${nameText}, age: ${ageText}, language: ${languageText}`;
}
getText('mike', 23, 'ko');
getText('mike', 23);
getText('mike', 23, 123); //에러 발생
```

여기서 language는 문자열도 가능하고 undefined일수도 있는것이다. ⇒ language?: string

그래서 코드에서는 undefined까지 고려해서 작성했다. ⇒ const languageText = language ? language.substr(0, 10) : '';

함수를 사용할때 language를 입력할수도 있고, 입력하지 않을수도 있지만, language에 숫자를 입력하려고 하면 에러가 날수 있다.

선택 매개변수 오른쪽에 필수 매개변수가 오면 컴파일 에러가 발생한다. ⇒ (name: string, language?: string, age: number)

함수를 호출할때 두번재 인수가 language를 가리키는지 age를 가르키는지 알수가 없기 때문이다.

```tsx
export{};

function getText(name: string, age: number =15, language = 'korean') : string {
    return `${name}, ${age}, ${language}`;
}

console.log(getText('mike'));
console.log(getText('mike', 23));
console.log(getText('jone', 36, 'english'));
```

위 코드의 결과는 아래와 같다.

```
mike, 15, korean
mike, 23, korean
jone, 36, english
```

이와 같이 함수를 정의할때 defualt값을 설정할수가 있다. 따로 변수를 적어넣지 않는다면 결과 값으로 defualt값을 사용한다.

위 코드에서 language의 타입 정의를 하지 않았다. 하지만 defualt값을 'korea'인 string으로 설정했기 때문에 따로 타입을 정의하지 않더라도 string으로 인식한다.

매개 변수로 rest parameter도 사용할수 있다.

```tsx
export{};

function getText(name: string, ...rest: number[]) : string {
    return `${name}, ${rest}`;
}

console.log(getText('mike', 1,2,3));
```

