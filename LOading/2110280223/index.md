---
layout: post
type: LOading
date: 2021-10-19 02:23
category: Typescript
title: 맵드타입에 대해서
subtitle: 이런것도 되다니...
writer: 000000
post-header: true
header-img: img/about.jpeg

hash-tag: [Typescrip, 맵드타입] 
---

맵드mapped 타입은 몇가지 규칙으로 새로운 interface를 만들수 있다.

맵드 타입은 기존 인터페이스의 모든 속성을 선택속성이나 읽기전용으로 만들 때 주로 사용된다.

- 설명에 들어가기 앞어 알아야할 키워드를 다시 되짚어 보자.

  ### keyof

  ------

  keyof는 Object의 key들의 lieteral 값들을 가져온다.

  오른쪽에 있는 interface의 모든 속성 이름을 나열한 것이다.

  아래 예시에서는 Person의 key값인 "name" | "age" 가 된다.

  "name" | "age" 이렇게 표기한 이유는 keyof가 유니온 타입으로 나열하기 때문이다.

  ```tsx
  interface Person {
      name: string
      age: number
  }
  
  type Test = keyof Person // ("name", "age")
  ```

  ### Union Type

  ------

  유니온 타입(Union Type)이란 자바스크립트의 OR 연산자(`||`)와 같이 `'A' 이거나 'B'이다` 라는 의미이다.



<br>

<br>

```tsx
export{};

//mapped type

interface Person {
    name : string;
    age : number;
}

interface PersonOptional {
    name? : string;
    age? : number;
}

interface PersonReadyOnly {
    readonly name : string;
    readonly age : number;
}
```

맵드타입으로 할수 있는 일을 몇가지 설명 하자면

인터페이스가 있을때 인터페이스에 있는 모든 속성을

optional(선택속성)로 바꾸거나 readOnly로 바꾸는 일을 맵드 타입을 이용해서 할 수가 있다.

<br>

맵드 타입을 이용한 코드를 살펴보자.

```tsx
export{};

type T1 = { [K in 'prop1' | 'prop2'] : boolean };
```

우선, 맵드 타입은 객체타입이기 때문에 중괄호가 보인다.

키 부분에는 대괄호가 있다. 대괄호 안에 in 이라는 키워드가 있는데 맵드 타입은 in 이라는 키워드를 사용한다.

왼쪽에 있는 K 는 아무거나 지정가능하다.

오른쪽에 있는 'prop1'|'prop2'가 중요한데, 두개의 문자열 리터럴을 유니온 타입으로 만들었다.

여기에 나열된 문자열이

이 전체 객체(타입)의 속성으로 만들어 진다.

형태는 저렇지만 실제 속성으로 만들어 지는 것을 확인하면,

```tsx
export{};

type T1 = { 
		prop1 : boolean;
		prop2 : boolean;
};
```

으로 만들어 진다.

이것이 맵드 타입의 문법이다.

<br>

<br>

## 맵드 타입의 활용(1) : Boolean

다음은 입력된 인터페이스의 모든 속성을 boolean 타입으로 만들어 주는 맵드 타입이다.

```tsx
export{};

interface Person {
    name : string;
    age : number;
}

//타입 작성
type MakeBoolean<T> = { [P in keyof T]?: boolean};

//실제 사용 코드
const pMap: MakeBoolean<Person> = {};
pMap.name = true;
pMap.age = false; //(false 대신 undefined도 입력 가능함)
```

MakeBoolean 이라는 타입을 만들었는데, 제네릭으로 만들었다.

T라는것을 입력 받아서 맵드 타입을 적용하는 것이다.

아마도 T라는 것은 주로 객체(타입)를 입력 받는것 같고 keyof를 사용했기 때문에 이 객체의 모든 속성 이름이 유니온 타입으로 나열이 될것이다.

그래서 T의 모든 속성에 대해서 boolean타입으로 만드는 것이다.

또한, 물음표를 사용했기 때문에 선택속성으로 만든다.

실제로 사용하는 코드를 보면

MakeBoolean 안에 Person을 입력 했다. 그러면 keyof를 하게 되면 name과 age가 유니온 타입으로 만들어 질것이고, 그래서 전체 속성에 대해서 물음표와 boolean이 적용이 될것이다. 그래서 이제 name과 age에 boolean값인 true와 false를 입력할수 있게 된 것이다.

interface를 통해 Person의 name과 age의 타입이 string과 number였는데 boolean이 된것이다. 선택속성이기 때문에 undefined도 입력할수가 있다.

대신 숫자는 입력할수가 없을 것이다.

<br>

<br>

## 맵드 타입의 활용(2) : readonly

```tsx
export {};

interface Person {
    name : string;
    age : number;
}

type T1 = Person['name'];
//타입 작성
type Readyonly<T> =  {readonly [P in keyof T]: T[P] };
type Partial<T> = { [P in keyof T]?: T[P]};

//실제 사용 코드
type T2 = Partial<Person>
type T3 = Readyonly<Person>
```

여기에 Readonly라는 맵드 타입을 정의를 하였다.

T라는 인터페이스를 입력 받아서 그 인터페이스에 keyof를 했기 때문에 모든 속성 이름을 나열을 했고, 그 모든 속성에 대해서 readonly를 붙여주는 것이다.

작성한 코드중 : T[P] 에 대해서 조금 더 알아보면

어떤 인터페이스에 속성 이름을 적어 주면 그 속성의 값의 타입을 의미한다. 여기서 T1을 보면 문자열이다. 왜냐면 Person의 name이 문자열이기 때문이다. 그래서 여기서는 모든 속성에 대해서 각 속성의 원래 값의 타입을 적어준 것이다. 값의 타입을 변화를 주지 않겠다는 것이다. 변화를 주는 것은 readonly를 붙여주는 것만 하는것이다.

밑에 Patial이라는 것은 위 코드와 비슷하긴 한데 readonly가 아니라 물음표를 입력을 했다. 선택 속성으로 만들어 주는 것이다. 모든 속성을 선택 속성으로 만들고 있고 마찬가지로 값의 타입은 변화를 주지 않는 것이다.

그래서 Partioal을 Person에 적용해 보면 모든 속성이 선택속성으로 변하게 된다.

반면, Readonly를 Person에 적용하게 되면 모든 속성에 readonly가 붙는다.

이렇게 맵드 타입을 이용하면 마치 함수를 사용하는 것처럼 사용을 할수가 있다. 일종의 유틸리티 타입이라고 보면 된다.

<br>

<br>

## 맵드 타입의 활용(3) : Pick

이번에는 Pick이라는 맵드 타입을 알아보자.

Pick은 interface에서 원하는 속성만 뽑아 낼수 있다.

```tsx
export {};

type Pick<T, K extends keyof T> = { [P in K] : T[P]};

interface Person {
    name : string;
    age : number;
    language : string;
}
type T1 = Pick<Person, 'name' | 'language'>;
```

여기서는 현재 두개의 제네릭을 입력 받고 있다.

먼저 인터페이스 T를 입력 받고 그다음 keyof T라고 했는데 이것은 T 라는 인터페이스에 있는 모든 속성 이름을 나열한 것이다. extends가 성립을 하려면 K는 keyof T에 할당 가능해야 한다. keyof T라는 것은 밑의 경우를 보면 T가 Person일때 keyof T는 name, age, language를 유니온 타입으로 묶어준게 된다.

이 K가 이것에 할당 가능하려면  이 세가지 이름중에 세가지 모두 입력을 하거나, 일부만 입력을 해야한다.

오른쪽 코드를 보면 in 오른쪽에 K가 있기 때문에 이렇게 입력된 K 속성들로 이루어진 인터페이스를 만드는 거고 값의 타입은 원래 타입을 사용하겠다라는 것이다.

그래서 T에는 Person을 입력하고 K에는 name과 language를 입력하게 되면 T1은 name과 language만 선택될 것이다. 원래 타입에서 age가 제외 되었다.

앞서 설명한 Readonly, Partial, Pick은 타입스크립트의 내장 타입이기 때문에 따로 정의를 하지 않아도 사용이 가능하다.

<br>

<br>

## 맵드 타입의 활용(4) : Record

이번에는 Record라는 타입을 알아보자.

```tsx
export {};

interface Person {
    name : string;
    age : number;
    language : string;
}
//코드 작성
type Record<K extends string, T> = { [ P in K]: T};

//실제 사용 코드
type T1 = Record<'p1' | 'p2', Person>;
```

Record는 문자열로 이루어진 K와 T를 입력을 받는다.

[P in K] 로 인해 P는 K 속성들로 이루어진 인터페이스를 만들고, 그 값들의 타입은 T로 하겠다는 것이다.

실제 사용코드를 보면 p1과 p2 속성으로 이루어진 인터페이스를 만드는데 값은 Person으로 하겠다라는 것이다.

그래서 T1을 보면

```tsx
type T1 = {
	p1: Person;
	p2: Person;
}
```

이렇게 만들어 진다.

<br>

<br>

## 맵드 타입의 활용(5) : enum 타입의 활용

맵드 타입을 사용하면 enum 타입의 활용도를 높일수 있다.

```tsx
export{};

enum Fruit {
    Apple,
    Banana,
    Orange,
}

const FRUIT_PRICE = {
    [Fruit.Apple] : 1000,
    [Fruit.Banana] : 1500,
    [Fruit.Orange] : 2000,
}
```

위 코드를 보면, Fruit이라는 enum타입이 있는데 이 모든 과일의 가격을 맵으로 관리를 한다면 enum 타입에 변화가 있을때 아래쪽 코드에도 수정이 필요하다. 하지만 코드를 작성하다 보면 enum 타입에서만 수정하고 아래쪽에서 수정을 빼먹는 경우가 발생한다.

맵드 타입으로 작성하면 enum타입에서 변화가 있을때 아래쪽 코드에 수정을 빠짐없이 입력 할수가 있다.

```tsx
//mapped type

const FRUIT_PRICE : { [keyof in Fruit] : number} = { 
    [Fruit.Apple] : 1000,
    [Fruit.Banana] : 1500,
    [Fruit.Orange] : 2000,
}
```

{ [keyof in Fruit] : number} 여기서 in 다음에 enum 타입을 작성해주면 enum에 있는 모든 아이템을 나열해 줘야 한다.
