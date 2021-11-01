---
layout: post
type: LOading
date: 2021-11-01 11:27
category: Typescript
title: 조건부타입에 대해서
subtitle: 보자보자 어디보자...
writer: 000000
post-header: true
header-img: img/about.jpeg

hash-tag: [Typescrip, 조건부타입] 
---

<br>

조건부 타입은 입력된 제네릭 타입에 따라 타입을 결정할수 있는 기능이다.

조건부 타입의 문법은 아래와 같이 쓰인다.

> T extends U ? X : Y

조건부 타입은 extends 키워드와 물음표 기호를 사용한다.

제네릭으로 입력된 어떤 타입 T가 U에 할당 가능하다면 X라는 타입을 사용하고, 그렇지 않으면 Y라는 타입을 사용하는 것이다.

```tsx
export{};

// T extends U ? X : Y

type IsStringType<T> = T extends string ? 'yes' : 'no';
type T1 = IsStringType<string>;
type T2 = IsStringType<number>;

//1.ts
```

위 코드를 보면

제네릭 T를 입력 받아서 이 T가 string에 할당 가능하다면 'yes'라는 문자열 리터럴이 사용되고,

그렇지 않으면 'no'가 사용된다.

헷갈릴수 있는게 자바스크립트의 삼항 연산자를 사용하듯이 사용을 하기 때문에 이것이 값을 다루는 것 처럼 보이지만 실제로는 값이 아니고 타입에 대해 얘기 하는것이다.

⇒ 'yes'라는 문자열 리터럴 타입을 얘기하는 것이다.

T1을 보면 string을 입력 했을 때는 string에 할당 가능하기 때문에 T1은 yes가 될것이고 yes라는 타입이 되는것이다.

T2를 보면 number를 입력했을때는 string에 할당 가능하지 않기 때문에 'no'가 되는 것이다.

조건부  타입은 유니온 타입과 함께 자주 사용된다.

```tsx
export{};

type IsStringType<T> = T extends string ? 'yes' : 'no';
type T1 = IsStringType<string | number>;
type T2 = IsStringType<string> | IsStringType<number>;

type Array2<T> = Array<T>;
type T3 = Array2<string | number>;

//2.ts
```

앞에서 살펴봤던 이런 조건부 타입 코드를 유니온 타입과 함께 사용을 하게 되면

원래 string 또는 number는 string에 할당 가능하지 않다.(string < string | number)

(string 만 쓸수 있는것 보다 string과 number를 같이 쓸수 있는 곳이 범위가 더 넓기 때문에)

하지만 T1에서는 'no'가 아니라 'yes'|'no' 가 된다.

조건부 타입 T에 유니온 타입을 사용 했을 때는 조금 특이하게 T1의 의미가 T2와 같아지게 된다.

조건부 타입 없이 T3처럼 제네릭을 단순하게 사용하면, 처럼 만들어 지지 않고 string 또는 number의 배열이 된다.

조건부 타입인 경우에만 유니온 타입이 입력되면 특이하게 이렇게 동작한다.

타입스크립트에는 Exclude 와 Extract라는 내장된 타입이 있다.

두타입 모두 조건부 타입으로 만들수가 있다.

```tsx
export {};

type T1 = number | string | never;

type Exclude<T, U> = T extends U ? never : T;
type T2 = Exclude<1|3|5|7, 1|5|9>;
type T3 = Exclude<string | number | (() => void), Function>;

type Extract<T,U> = T extends U ? T : never;
type T4 = Extract<1|3|5|7, 1|5|9>;

//3.ts
```

먼저 유니온 타입에 never는 제거가 되는데 이는 이런 조건부 타입에서 자주 사용되는 개념이다.

Exclude를 보면 T와 U를 입력을 받아서 T가 U에 할당 가능하다면 never를 사용하고,

그렇지 않으면 T를 그대로 사용하는 타입니다.

그래서 사용하는 코드를 보면 1,5,9에 할당 가능한 것을 제외한다. 그리고 유니온 타입으로 입력 됫을 때는 각각 적용이 된다고 위에서 언급했다.

그래서 보면, 1 은 1|5|9 에 할당 가능하다. 그래서 제외가 된다.

이런식으로 보면 제외가 되지 않는 것은 3과 7이기 때문에 T2의 타입은 3 또는 7이 된다.

그 아래 코드를 보면 Function에 할당 가능한 것이 제외될 것이기 때문에 세번째 타입이 제거가 될것이고, String 또는 number만 남게 된다. 그래서 T3의 타입은 string | number이다.

Extract 함수를 보면 반대로 되어 있다. 할당가능하면 T 가 사용되고  안되면 never가 된다.

그래서 T4 는 1 | 5 가 된다.

<br>

<br>

## ReturnType

타입스크립트에는 ReturnType 이라는 내장 타입이 있다.

ReturnType은 조건부 타입으로 만들어 졌다.

```tsx
export{};

type ReturnType<T> = T extends (...args: any[]) => infer R ? R:any;
type T1 = ReturnType<() => string>;
function f1(s: string) : number {
    return s.length;
}
type T2 = ReturnType<typeof f1>;

//4.ts
```

ReturnType이 하는 일은 T가 함수 일때  T함수의 반환 타입을 뽑아 준다.

`(...args: any[]) => infer R ? R:any;` 코드를 살펴보면 T라는 것이 함수에 할당 가능한 타입이면 R 이라는 것을 사용하겠다는 것이다.

여기서 R은 함수의 반환 타입을 의미한다.

이때 타입 추론을 위해 infer라는 키워드를 사용하였다. infer 덕분에 함수의 반환 타입을 R이라는 변수에 담을수 있다.

infer 키워드는 조건부 타입을 정의 할때 extends 키워드 뒤에서 사용된다.

그래서 ReturnType을 사용하는 곳을 보면 지금 `() => string` 를 입력 했을때 반환 타입은 string 이기 때문에 T1은 string이 된다.

T2에서는 f1이라는 함수를 사용 했는데 f1은 실존하는 값이다. 제네릭에는 타입만 입력할수 있기 때문에 값으로부터 타입을 가져오기 위해서 `typeof` 라는 키워드를 사용할수 있다.

`typeof f1` 을 하면 f1의 타입이 되는것이고 f1 함수는 number를 반환하기 때문에 T2는 number가 된다.

<br>

<br>

## infer 키워드 활용

앞에서 봤던 infer키워드를 좀더 알아보자.

앞서 infer키워드는 조건부 타입을 정의할때 extends 키워드 뒤에서 사용 된다고 하였다.

```tsx
export {};

type Unpacked<T> = T extends (infer U)[]
    ? U
    : T extends (...args : any[]) => infer U
    ? U
    : T extends Promise<infer U>
    ? U
    : T;
type T0 = Unpacked<string>;
type T1 = Unpacked<string[]>
type T2 = Unpacked<() => string>;
type T3 = Unpacked<Promise<string>>;
type T4 = Unpacked<Promise<string>[]>;
type T5 = Unpacked<Unpacked<Promise<string>[]>>;

//5.ts
```

infer 키워드는 중첩해서 사용할수가 있다.

우선 `type Unpacked` 를 한번 살펴보자.

`T extends (infer U)[]` : 입력된 <T>라는 타입이 어떤 값의 배열이면 그 배열의 아이템(타입)을 사용하겠다 라는 것이다.

여기서 어떤 배열인지는 아직 결정 되지 않았기 때문에 infer 키워드를 사용한 것이다.

그런데 만약 배열이 아니라면 아래쪽 코드로 내려오게 된다.

`: T extends (...args : any[]) => infer U` : 그래서 T 가 함수에 할당 가능한 타입이라면 이 함수의 반환 타입을 사용하겠다 라는 것이다.

만약 함수에 할당 가능한 타입이 아니라면 다시 아래쪽 코드로 내려와서

`: T extends Promise<infer U>` : Promise에 할당가능한 타입이라면 이 Promise의 값인 U를 사용하겠다 라는 것이다.

여기서도 마찬가지로 Promise에 어떤 값인지는 아직 결정되지 않았기 때문에 infer 키워드를 사용한 것이다.

`: T;` : 위 세 경우 모두 만족하지 않으면 T 자기 자신을 사용한다.

그래서 사용하는 코드를 살펴보면

T0 ⇒ string이 입력 됐을 때는 세가지 다 아니기 때문에 자기자신인 string이 반환

T1 ⇒ string에 배열이 입력되면 첫번째 조건에 따라서 이 배열의 아이템(타입)인 string이 반환

T2 ⇒ 함수가 입력이 되면 함수가 반환한 string이 반환

T3 ⇒ Promise가 입력되면 Promise의 값인 string이 반환

T4 ⇒ Promise의 배열이 입력 되면 첫번째 조건에 걸려 먼저 Promise가 반환

T5 ⇒ Unpacked를 두번 사용하게 되면 처음 배열 조건에 따라 Promise가 반환이 되고, 그다음 세번째 조건에 따라서 string이 반환이 될것이다.

<br>

<br>

이번에는 조번부 타입을 사용하여 몇가지 util 함수를 만들어 보자.

```tsx
export {};

type StringPropertyNames<T> = {
    [K in keyof T] : T[K] extends string ? K : never;
}[keyof T];
interface Person {
    name : string;
    age : number;
    nation : string;
}
type T1 = StringPropertyNames<Person>

//6.ts
```

`StringPropertyNames` 타입은 인터페이스에서 값이 문자열인 속성 이름을 추출하는 유틸리티 타입이다.

- `{[K in keyof T] : T[K] extends string ? K : never;}`

  이 부분을 먼저 살펴보면, 맵드타입으로 정의가 되어 있다.

  `in`이라는 키워드가 사용 됐고 `keyof T` 라고 했기 때문에 여기서 T는 인터페이스 이고 그 인터페이스의 속성 이름을 유니온 타입으로 나열한다.

  그 모든 속성 이름에 대해서 자기자신의 값의 타입(`T[K]`)이 string에 할당 가능하다면 K를 사용한다라는 것이다. *(여기서 K는 key이기 때문에 속성 이름이 된다.)*

  그렇지 않으면 never를 사용한다.

  그래서 `StringPropertyNames` 타입에 <Person>을 입력하면,

  아래와 같은 타입이 만들어 진다.

  ```tsx
  interface Person2 {
      name : 'name';
      age : never;
      nation : 'nation';
  }
  ```

  name과 nation이 string이기 때문에 string 인것만 그 속성 이름이 되는것이고 그렇지 않은것은 never가 된다.

- `[keyof T]`

  그다음 대괄호를 입력을 했다. 대괄호를 입력하는 것은 이 속성 이름에 해당하는 값의 타입을 뽑아내는 것이다.

  name이라는 속성의 값의 타입은 문자열 리터럴 'name', nation의 속성의 값의 타입은 문자열 리터럴 'nation', age의 속성의 값의 타입은 never(유니온에서 제외) 이다.

  ⇒ 즉 T의 모든 속성 이름에 대해서 적용을 했기 때문에 never는 제거가 되고 문자열인 속성 이름만 나열이 되는것이다.

- `type T1 = StringPropertyNames<Person>` ⇒ `type T1 = "name" | "nation"`

```tsx
type StringProperties<T> = Pick<T, StringPropertyNames<T>>;
type T2 = StringProperties<Person>

//
type T2 = {
			name : string ;
			nation : string ;
}
```

이어서 `StringPropertyNames` 타입을 활용해서 StringProperties라는 것을 정의해 보면

Pick 을 사용하였고 T 라는 인터페이스에서 문자열 속성 이름을 유니온 타입으로 뒤에 적어줬기 때문에

T2는 결론적으로 String으로만 된 타입이 된다.

이번에는 Omit이라는 타입을 알아보자.

```tsx
export{};

type Omit<T, U extends keyof T> = Pick<T, Exclude<keyof T, U>>;

interface Person {
    name: string;
    age: number;
    nation: string;
}
type T1 = Omit<Person, 'nation' | 'age'>;

//8.ts
```

이 타입도 타입스크립트에 내장된 타입니다.

코드를 보면 Person 이라는 인터페이스에서 nation과 age를 제거한다 라는 의미이다.