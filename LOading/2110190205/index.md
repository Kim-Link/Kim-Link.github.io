---
layout: post
type: LOading
date: 2021-10-19 02:05
category: Typescript
title: 제네릭에 대해서
subtitle: 지금까지 이런 것은 없었다...
writer: 000000
post-header: true
header-img: img/about.jpeg

hash-tag: [Typescrip, 제네릭, 동적] 
---



지금까지의 타입 결정은 정적이었다.

이거는 이 타입, 저거는 저 타입 이런식으로.

하지만 똑똑한 타입 스크립트는 타입이 동적으로 작동하도록 제공한다.

이거는 이 타입 인데 저 타입 이기도 하고 이런식으로.

이것을 가능하게 한 기능이 제네릭이다.

몇가지 특성들로 제네릭을 확인해보자.

<br><br>

### 중복코드 제거

------

제네릭은 타입 정보가 동적으로 결정되는 타입이다.

제네릭을 통해 같은 규칙을 여러 타입에 적용할 수 있기 때문에

타입 코드를 작성할 때 발생할 수 있는 중복코드를 제거할수 있다.

```tsx
export {};

function makeNumberArray(defaultValue : number, size : number) : number[] {
    const arr: number[] = [];
    for( let i = 0 ; i < size; i++){
        arr.push(defaultValue);
    }
    return arr;
}
function makeStringArray(defaultValue : string, size : number) : string[] {
    const arr: string[] = [];
    for( let i = 0 ; i < size; i++){
        arr.push(defaultValue);
    }
    return arr;
}
const arr1 = makeNumberArray(1, 10);
const arr2 = makeStringArray('empty', 10);
```

위 코드를 보면 makeNumberArray 함수와 makeStringArray 함수의 로직은 거의 똑같다.

위에 함수가 하는 일은 number array를 반환을 하는데 입력된 size 만큼의 배열을 반환한다.

모든 값은 defaultValue로 초기화를 하고 있다.

아래 함수가 하는 일도 비슷한 역할을 하는데 다른점은 배열에 들어가는 값이 string이라는 것만 다르다.

이 함수들을 사용할때는 필요한 타입에 따라 함수를 호출해주면 된다.

숫자와 문자열만 필요하다면 다음과 같이 함수 오버로드(overload)를 사용하면 된다.

```tsx
export{};

function makeArray(defaultValue : number, size : number) : number[];
function makeArray(defaultValue : string, size : number) : string[];
function makeArray(defaultValue : boolean, size : number) : boolean[]; //bolean타입을 추가 하기 위해 작성
function makeArray(
    defaultValue : number | string | boolean,
    size: number | string | boolean,
) : Array<number | string  | boolean> {
    const arr: Array<number | string  | boolean> = [];
    for (let i = 0; i < size; i++) {
        arr.push(defaultValue);
    }
    return arr;
}
const arr1 = makeArray(1,10);
const arr2 = makeArray('empty', 10);
```

먼저 필요한 타입을 정의해주고, 아래 로직을 작성하는 방법이다. makeNumberArray 함수와 makeStringArray 함수 코드와 비교해 보면 로직의 중복이 없어졌고 하나의 함수로 사용이 가능해 졌다.

makeArray함수의 경우 숫자를 입력했을때 반환값은 자동으로 숫자가 되고, 문자열로 입력했을때는 자동으로 반환값이 문자열이 된다.

반면, 이렇게 작성했을때 문제점도 있는다.

makeArray함수에서 타입을 추가하고 싶으면 코드도 추가해야 한다. 추가해야 하는 타입이 들어날수록 번거로워 진다.

그렇기 때문에 제네릭을 사용하면 이렇게 작성 가능하다.

```tsx
export{};

function makeArray<T>(defaultValue : T, size : number): T[] {
    const arr: T[] = [];
    for (let i = 0; i < size; i++) {
        arr.push(defaultValue);
    }
    return arr;
}
const arr1 = makeArray<number>(1,10);
const arr2 = makeArray<string>('empty', 10);
const arr3 = makeArray(1,10);
const arr4 = makeArray('empty', 10);
```

제네릭은 함수이름 오륵쪽에 <>를 이용해서 입력할수 있다.

여기서 T라는 것은 원하는 이름으로 정할 수 있고 현재 **T라는 것의 타입은 정해지지 않은것** 이다.

T의 타입은 나중에 동적으로 결정될것이고, 이 T는 매개변수 쪽과 구현하는쪽 모두 사용할수가 있다.

아래쪽에 cosnt arr1을 보면 <number>로 타입을 적은것을 확인할 수 있다.

추가적으로 arr3, arr4 를 보면 제네릭을 사용했을때 따로 타입을 정해주지 않아도 타입스크립트에서 자동으로 타입을 인식한것을 확인할수 있다.

자동으로 인식하는것이 마음에 들지 않는다면 <>를 사용하여 명시적으로 입력해주면 된다.

<br>

### 타입의 다양성 지원

------

제네릭은 데이터의 타입에 다양성을 부여해주기 때문에 자료구조에서 많이 사용된다.

아래 코드는 제네릭을 사용하여 Stack Class를 구현한것이다.

```tsx
export{};

class Stack<D> {
    private items: D[] = [];
    push(item: D) {
        this.items.push(item);
    }
    pop() {
        return this.items.pop();
    }
}

const numberStack = new Stack<number>();
numberStack.push(10);
const v1 = numberStack.pop();
const stringStack = new Stack<string>();
stringStack.push('a');
const v2 = stringStack.pop();

let myStack: Stack<number>;
myStack = numberStack;
myStack = stringStack; //에러 발생
```

이번에도 Stack 옆에 <D>로 제네릭을 입력했다.

items라는 배열의 타입을 D의 배열로 정의를 했고, push할때는 D라는 타입의 item을 받아서 그대로 push한다.

pop할때는 그대로 pop을 하고 있다.

numberStack 코드를 보면 number의 스텍을 만들어 놓고 push할때는 number를 push 했다.

pop을 할때 타입을 보면 number 또는 undefined가 된다. pop이 undefined를 반환할 수도 있기 때문이다.

만약 let myStack처럼 타입만 정의해 놓고 할당을 하는 코드를 보면

numberStack만 할당 할 수 있고, stringStack은 할당 할 수 가 없다.

<br>

### extends 키워드의 활용

------

타입스크립트 안에서 제네릭을 사용할때, 입력가능한 값의 제한이 없었다.

반면, 리액트와 같은 라이브러리의 API는 입력가능한 값의 범위를 제한한다.

예를 들어 리액트의 속성값 전체는 객체 타입만 허용이 된다.

이를 위해 타입스트립트의 제네릭은 타입의 종류를 제한할 수 있는 기능을 제공한다.

이때 사용되는 것이 extends라는 키워드 이다.

```tsx
export {};

function identity<T extends number | string>(p1: T): T {
    return p1;
}
identity(1);
identity('a');
identity([]); //에러 발생
```

여기서는 T 의 타입이 number 또는 string이라고 정의를 한것이다.

extends를 쉽게 설명하면

A extends B ⇒ 'A가 B에 할당 가능해야 한다'

로 이해하면 된다.

여기서 어떻게 A가 B에 할당 가능할까?

조건이 많으면 범위가 좁다. 즉, B에서 확장하여 A는 더 많은 조건을 가지게 되었으므로, 더 적은 조건을 가지고 있는 B에 A가 할당 가능해야 한다.

조금 더 쉽게 설명하자면, A는 B로 부터 상속받은 개념이므로, A는 B가 가지고 있는 모든 조건을 가지고 있게 되는 것이다.

<br>

위 코드에서

T extends number | string ⇒ ' T가 number 또는 string에 할당 가능해야 한다'

로 볼수 있다.

그래서 숫자나 문자열을 입력할수 있지만, 배열이 들어오게 되면 에러가 발생한다.

<br>

extends의 활용에 대해 좀더 알아 보자.

```tsx
export{};

interface Person {
    name : string;
    age : number;
}

interface Korean extends Person {
    liveInSeoul : boolean;
}
type T1 = keyof Person;
function swapProperty<T extends Person, K extends keyof Person>(
    p1: T,
    p2: T,
    key: K,
): void {
    const temp = p1[key];
    p1[key] = p2[key];
    p2[key] = temp;
}

const p1 : Korean = {
    name : '홍길동',
    age : 23,
    liveInSeoul : true,
}
const p2: Korean = {
    name: '김삿갓',
    age : 31,
    liveInSeoul: false,
};
swapProperty(p1, p2, 'age')
```

위 코드를 보면

먼저 Person이라는 interface로 타입을 정의 했고,

Korean은 Person을 확장해서 다시 interface로 타입을 정의 했다.

swapProperty함수에서 Person을 확장해서 T를 정의 했고, keyof Person을 확장해서 K를 정의했다.

keyof 라는 것은 오른쪽에 있는 interface의 모든 속성 이름을 나열한 것이다. 여기서는 "name" | "age" 타입인 것이다.

swapProperty함수가 하는 일은 두개의 객체와 하나의 KEY를 입력을 받아서 키의 해당하는 값을 서로 바꿔주는 것이다.

```tsx
export{};

interface Person {
    name : string;
    age : number;
}

interface Korean extends Person {
    liveInSeoul : boolean;
}
//type T1 = keyof Person;
function swapProperty<T extends Person, K extends keyof Person>(
    p1: T,
    p2: T,
    key: K,
): void {
    const temp = p1[key];
    p1[key] = p2[key];
    p2[key] = temp;
}

interface Product {
    name : string;
    price : number;
}

const p1 : Product = {
    name : '시계',
    price: 1000,
}
const p2: Product = {
    name: '자전거',
    price : 2000,
};
swapProperty(p1, p2, 'name') //에러 발생
```

위 코드에서 에러가 발생하는 이유는

p1이 Product 타입을 받았는데, swapProperty에서는 객체의 타입을 Person이 들어가야 하기 때문에 에러가 발생한 것이다.

