---
layout: post
type: LOading
date: 2021-10-17 21:40
category: Typescript
title: 타입 추론
subtitle: 타입스크립트 넌 정말...
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Typescrip, 타입 추론] 
---



타입스크립트는 타입 추론을 제공해준다.

그렇기 때문에 꼭 필요한 경우에만 타입 정의를 할 수 있다.

타입 추론 덕분에 타입을 일일히 다 치지 않아도 높은 수준의 타입 안정성을 유지할수 있다.

   

### let의 타입추론

------

```tsx
export{};

let v1 = 123;
let v2 = 'abc';
v1= 'a'; //에러 발생
v2 = 456; //에러 발생

//1.ts
```

위 코드는 타입스크립트에서 그냥 자바스크립트로 작성한 코드이다.

보면 타입을 따로 정의 하지 않았지만 타입 추론으로 인해 let으로 선언하고 할당한 뒤 재할당 할때 처음 할당했던 타입과 다르면 에러가 발생하는것을 확인할수 있다.

   

### const의 타입추론

------

```tsx
export{};

const v1 = 123;
const v2 = 'abc';
let v3 : typeof v1 = 234; //에러발생

//2.ts
```

let 으로 선언한 변수는 재할당 가능하기 때문에 융통성 있게 타입이 결정 된다.

반면 const 변수는 값이 변하지 않기 때문에 let 변수 보다 타입이 엄격하게 관리 된다.

지금 const 변수 v1에 123을 넣었는데, 이때 타입은 number 가 되는것이 아니라 123이 된다.

그리고 v2는 타입이 'abc'가 된다.

typeof 는 값으로 부터 타입을 뽑아 낼수가 있다. 지금 v1의 타입으로 v3를 정의 했는데 에러가 발생했다.

v1의 타입이 number가 아닌 123이기 때문이다.

   

### 배열과 객체의 타입 추론

------

```tsx
export{};

const arr1 = [10, 20, 30];
const [n1, n2, n3] = arr1;
arr1.push('a');//에러 발생

const obj = { id : 'abcd', age : 123, language : 'korean'};
const { id, age, language } = obj;
console.log(id===age); //에러 발생
```

arr1 이라는 변수에 타입을 정의하지 않았지만 그냥 숫자로 이루어진 배열을 할당하면 자동으로 number의 배열이 된다.

숫자 배열로 정의가 되었기 때문에 문자열을 push 하려고 하면 에러가 발생한다.

객체의 경우에도 따로 타입을 정의하지 않았지만 자동으로 타입 추론이 된다.

배열과 객체 모두 비구조화 할당을 해도 타입추론으로 인해 타입이 정의된다.

   

### interface에서의 타입추론

------

```tsx
export{};

interface Person {
    name : string;
    age : number;
}
interface Korean extends Person {
    liveInSeoul: boolean;
}
interface Japanese extends Person {
    liveInTokyo: boolean;
}

const p1: Person = { name: 'mike', age : 23};
const p2: Korean = { name: 'mike', age : 25, liveInSeoul: true};
const p3: Japanese = { name: 'mike', age : 27, liveInTokyo: false};
const arr1 = [p1, p2, p3];
const arr2 = [p2, p3];

//4.ts
```

코드를 보면 3개의 interface가 있다.

Person에 이를 확장해서 Korean과 Japanese를 만들었다.

이후 변수 3개를 만들어서 타입을 각각 하나씩 할당하고, 값을 다르게 할당하였다.

그다음 배열 2개를 만들었다.

arr1의 타입을 확인해 보면 타입이 Person으로 통합되어 있다. ⇒ 다른 타입으로 할당 가능한 타입은 제거가 된다.

Korean과 Japanese는 둘다 Person에 할당 가능하기 때문에 제거가 되고 Person만 남았다.

arr2의 타입을 확인해 보면 타입이 각각 Korean과 Japanese가 유니온으로 결정 되어 있다. ⇒ 이 두개의 타입은 서로 할당 관계에 있지 않기때문에 서로 제거할수가 없다.

여러가지 타입이 남으면 유니온으로 묶여 있게 된다.

   

### 함수의 타입 추론

------

```tsx
export {};

function func1(a = 'abc', b = 10) {
    return `${a}${b}`;
}
func1(3,6);//에러발생
const v1: number = func1('a', 1);//에러 발생

function func2(value: number) {
    if(value < 10) {
        return value;
    } else {
        return `${value} is too big`;
    }
}
```

func1에서 본것 처럼 기본값만 입력해도 자동으로 타입이 정의된다.

그리고 결과 까지도 함수타입이 추론된다.

그래서 func1의 첫번째 변수에 숫자를 넣으면 에러가 발생한다.

const v1에서는 함수의 반환값이 string이기 때문에 number에 할당할수 없다고 에러가 나고 있다.

func2에서는 return이 각각 다른 타입을 반환 하고 있다. 이런 경우에도 타입 추론은 가능하다.
