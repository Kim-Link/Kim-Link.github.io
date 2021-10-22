---
layout: post
type: LOading
date: 2021-10-17 21:40
category: Typescript
title: 타입 가드
subtitle: 타입스크립트 똑또케
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Typescrip, 타입 가드] 
---



### 타입가드란?

:  조건문에서 자동으로 타입의 범위를 좁혀주는 타입스크립트의 기능이다.

### as 키워드

------

as 키워드는 앞에 있는 값의 타입을 개발자가 확신하는 경우에 그 타입을 적어줌으로써 뒤에 있는 타입을 강제 하는 기능이다.

```tsx
export{};

function print(value : number | string) {
    if (typeof value === 'number') {
        console.log((value as number).toFixed(2));
    } else {
        console.log((value as string).trim());
    }
}

//1.ts
```

타입 스트립트는 number라고 추론하고 있지 않지만 사용자가 number로 확신하는 경우 타입을 강제하기 위해서 as number 라고 사용한다.

as는 반드시 어쩔수 없는 경우에만 사용해야 한다. ⇒ 많이 사용하면 버그의 위험도가 높아진다.

위 코드에서 한가지 특이한 점은 typeof 키워드를 값의 영역에서 사용했다.

이전까지는 타입의 영역에서 typeof를 사용했었다.

값의 영역에서 typeof를 사용하게 되면 뒤에 있는 값의 타입을 문자열로 반환해 준다.

타입 가드 기능 덕분에 아래와 같이 사용해도 같은 코드가 된다.

```tsx
export{};

function print(value : number | string) {
    if (typeof value === 'number') {
        console.log((value).toFixed(2));
    } else {
        console.log((value).trim());
    }
}

//1.ts
```

이렇게 값의 영역에서 사용한 코드를 분석해서 타입의 범위를 좁혀주는 기능이 타입 가드 이다.

### class의 경우

------

class의 경우에는 instanceof 라는 키워드를 사용할수가 있다.

instanceof는 자바스크립트에 있는 기능이다.

```tsx
export{};

class Person {
    name : string;
    age : number;
    constructor(name: string, age: number) {
    }
}
class Product {
    name : string;
    price : number;
    constructor(name: string, age: number) {
    }
}
function print(value: Person | Product) {
    console.log(value.name);
    if(value instanceof Person) {
        console.log(value.age);
    } else {
        console.log(value.price);
    }
}
```

아래 함수에서 instanceof 앞에 있는 값이 뒤에 있는 클래스의 객체인지 검사를 한다.

지금 value는 Person 아니면 Product이기 때문에

if문 안에 들어 왔다는 것은 value가 Person의 객체라는 것이고

else 문 안에 들어 왔다는 것은 value가 Product의 객체라는 것이다.

⇒ 타입 가드가 동작을 한다.

```tsx
export{};

interface Person {
    name: string;
    age: number;
}
interface Product {
    name: string;
    price : number;
}
function print(value: Person | Product){
    if(value instanceof Person) { //에러 발생
        console.log(value.age); //에러 발생
    } else {
        console.log(value.price); //에러 발생
    }
}
//4.ts
```

지금 위 코드에서는 Person과 Product라는 인터페이스가 있고 클래스는 없다.

그런데 instanceof 뒤에 interface를 사용하려고 하면 에러가 난다.

interface는 컴파일 한뒤 사라지는 코드인데, instaceof 는 값의 영역에서 사용된다.(컴파일 후에 남아 있음)

때문에 컴파일 이후에 없는 Person을 가지고 오지 못하기 때문에 에러가 발생한다.

⇒ 결국 instaceof 뒤에는 class나 생성자 함수가 올수 있다.

### interface 구별하기

------

inteface를 구별하기 위해서는 식별가능한 유니온(discrminated union) 타입을 사용해야 한다.

```tsx
export{};

// discrminated union

interface Person {
    type : 'a';
    name : string;
    age : number;
}
interface Product {
    type : 'b';
    name : string;
    price : number;
}
function print(value: Person | Product) {
    if (value.type === 'a') {
        console.log(value.age);
    } else {
        console.log(value.price);
    }
}

//5.ts
```

같은 이름의 속성을 정의하고 속성의 타입은 모두 겹치지 않게 정의한다.

위 코드에서 Person과 Product 모두 type이라는 이름의 속성을 정의 하고

그 타입을 서로 겹치지 않게 정의를 해두었다.

밑에 함수에서 사용할때 `value: Person | Product` 로 정의를 하고

if문 에서 이 타입이 a인지 검사를 하고 있다.

지금 우리가 as를 사용하지 않았지만 Person에만 있는 age라는 속성을 사용하려고 할때 에러가 나지 않는다.

이것은 타입가드가 동작을 하기 때문에 타입스크립트가 value가 Person타입이라는 것을 알기 때문이다.



### switch 문에서의 사용

------

식별 가능한 유니온 타입은 서로 겹치지 않기 때문에 switch문에서 사용하기 좋다.

```tsx
export{};

interface Person {
    type : 'a';
    name : string;
    age : number;
}
interface Product {
    type : 'b';
    name : string;
    price : number;
}

function print(value: Person | Product) {
    switch (value.type) {
        case 'a':
            console.log(value.age);
            break;
        case 'b':
            console.log(value.price);
            break;
    }
}
```

switch 문의 조건으로 타입 속성값을 입력하면

각각 case에서 해당 값을 구분해서 사용할수가 있다.

### 타입을 검사하는 함수

------

타입을 검사하는 함수를 작성하는 방법이 있다.

```tsx
export{};

interface Person {
    name : string;
    age : number;
}
interface Product {
    name : string;
    price : number;
}

function isPerson(x: Person | Product) : x is Person {
    return (x as Person).age !== undefined;
}
function print(value : Person | Product) {
    if (isPerson(value)) {
        console.log(value.age);
    } else {
        console.log(value.price);
    }
}
```

이전과 다르게 지금 Person과 Product에 type이라는 속성이 없다.

눈으로 봤을때 Person에는 age가 있고, Product에는 price가 있기 때문에 그것으로 구분 할수가 있을것 같다.

그래서 isPerson 함수에서는 그런 방식으로 처리를 하고 있다.

함수의 반환 타입을 보면 is라는 키워드를 사용하였다.

is 뒤에 타입 이름을 적어준다.(지금은 Person을 검사하기 때문에 Person을 적었다.)

매개변수 x는 Person 혹은 Product이고, x 안에 age라는 속성이 있다면 Person이 된다.

이제 isPerson 함수를 if문 안에 사용해서 타입을 알수 있다.

⇒ 에러가 발생하지 않는것으로 보아 타입 가드가 적용된것을 확인할수 있다.

위와 같이 함수를 작성하는게 번거롭다면, in이라는 키워드를 사용할수 있다.

```tsx
export{};

interface Person {
    type : 'person';
    name : string;
    age : number;
}
interface Product {
    type : 'product'
    name : string;
    price : number;
}

function print(value : Person | Product) {
    if ('age' in value) {
        console.log(value.age);
    } else {
        console.log(value.price);
    }
}
```

in이라는 키워드는 자바스크립트에 있는 기능이다. 어떤 속성이 있는지 검사하는 기능이다.

속성이 있는지 검사하는 것으로 타입 가드가 동작을 한다.

사실 식별 가능한 유니온 타입 보다 속성 이름을 검사하는 방법이 좀더 간편하다.

하지만,

타입의 종류가 많아지고 같은 이름의 속성이 중복으로 사용된다면 이런 방법을 사용하기 힘들기 때문에 식별 가능한 유니온 타입을 사용한다.