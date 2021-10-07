---
layout: post
type: LOading
date: 2021-10-06 17:30
draft: false
category: Typescript
title: 타입스크립트가 무엇인가
subtitle: JavaScript Superset
writer: 000000
post-header: true
header-img : img/about.jpeg
hash-tag: [Javascript, Typescript]
---



---



### 타입스크립트?

자바스크립트에 type과 관련한 문법을 추가로 적용한 것을 타입스크립트라고 한다.

마이크로소프트에서 개발하였으며, MS 자체에서도 **Javascript Superset**으로 정의하고 있다.



### 왜 만들어졌는가?

자바스크립트는 Dynamic Typing이 가능하다.

예를 들어 ` 5 - '3' `을 하게 되면 일반적으로는 숫자-문자 이기 때문에 오류가 발생해야 하지만, 자바스크립트는 적당히 알아서 연산을 처리해서 알아서 숫자를 바꿔준다.

이러한 '유연성'이 소규모 프로젝트에서는 오류를 다소 줄여주는 등 도움이 되기도 하지만, 프로젝트가 클 경우 장점은 소멸하고 치명적인 단점이 된다. (버그를 찾기 더욱 어려워지기 때문)

이 때문에 타입을 엄격히 검사하고, 에러메세지 또한 디테일하게 알려주는 타입스크립트를 이용하면 예상치 못한 버그를 쉽게 찾고 고칠 수 있다. (심지어 타입스크립트는 오타 교정도 해준다)





### 설치법

> **Static 개발 시**
>
> 1. `Node.js` 최신버전 설치 (최신버전이 아니면 오류가 발생함)
>
> 2. 터미널에 `npm install -g typescript` 입력 (오류 발생 시 `Node.js` 설치가 제대로 안 되어 있을 가능성 높음)
>
> 3. 코드가 저장될 폴더를 만들고, 에디터로 폴더를 오픈
>
> 4. `파일명.ts` 와 같은 ts확장자 파일 생성
>
> 5. `tsconfig.json` 파일 생성
>
> 6. `tsconfig.json`파일에 아래 코드 입력
>
>    ```json
>    {
>      "compilerOptions": {
>        "target": "es6",
>        "module": "commonjs"
>      }
>    }
>    ```
>
>    - target은 컨버팅할 js 문법 버전을 입력하면 된다. es3, es5, es6, esnext(최신) 등 정의 버전에 따라 컨버팅 문법이 바뀐다.
>
> 7. 터미널에 `tsc -w`를 입력하고 켠 상태 유지 (js 컨버팅을 자동으로 유지해 줌)
>
> 8. 작업 완료 후 js파일을 브라우저에서 로드하도록 하면 끝

> **이미 있는 React 프로젝트에 설치**
>
> 터미널에 `npm install --save typescript @types/node @types/react @types/react-dom @types/jest` 입력.
>
> 터미널 코드가 실행되면 js파일을 ts로 변경하여 사용할 수 있음.

> **React 프로젝트 신규 생성**
>
> 터미널에 `npx create-react-app my-app --template typescript` 입력. (대시 두개 입력하는거 하나 하면 안될수도 있음)

> **Vue 프로젝트 신규 생성**
>
> 1. 작업 폴더 경로에서 터미널을 오픈한 뒤 `vue add typescript`를 입력하면 라이브러리가 설치됨
>
> 2. vue 파일에서 타입스크립트를 사용하려면
>
>    ```vue
>    <script lang="ts">
>    </script>
>    ```
>
>    이렇게 lang 옵션을 켜 둔채 사용하면 됨. 또한, vue 프로젝트 내에서도 tsconfig.json파일을 만들어서 자유롭게 설정할 수도 있음.





### 타입스크립트 기본 문법

- 변수를 만들 때 타입 지정이 가능하다. 타입을 지정해 놓으면 의도치 않게 변경될 경우 에러메세지를 띄운다.

  ```typescript
  let name :string = 'kim'
  ```

  변수명:타입명 형식으로 작성하며, 타입으로 쓸 수 있는 것들은 아래와 같다

  - string
  - number
  - boolean
  - bigint
  - null
  - undefined
  - []
  - {}

  

- array 혹은 object 자료는 아래와 같이 타입 지정이 가능하다.

  ```typescript
  let arr :string[] = ['kim-link', 'tess']
  let obj :object{ grade : number } = { grade : 1 }
  ```



- 변수에 여러가지 타입의 데이터가 들어올 수 있다면 `|`(or) 기호를 이용해 연산자를 표현할 수 있다.

  ```typescript
  let group : string | number = '1'
  ```



- type 키워드를 이용해 타입을 변수처럼 담아서 사용할 수 있다.

  ```typescript
  type nameType = string | number;
  let name :nameType = 'jimin';
  ```

  

- type 키워드를 이용해 나만의 타입을 만들어 사용할 수 있다. (literal type)

  ```typescript
  type genderType = 'male' | 'female'
  let gender :genderType = 'female' // 여기는 male과 female이 아닌 값이 들어 올 경우 모두 오류로 처리
  ```



### 타입스크립트 함수 문법

- 함수는 파라미터와 return 값에 대한 타입 지정이 가능하다. 그리고 **return 타입으로 void를 설정할 수 있는데, return이 없는 지 체크할 수 있는 타입이다**

  ```typescript
  function hamsu(x :number) :number{ // return 타입 여기서 지정해야 함
    return x * 2
  }
  ```



- 타입스크립트는 **현재 변수의 타입이 확실하지 않으면 마음대로 연산할 수 없다**. 항상 타입이 무엇인지 미리 체크하는 `narrowing`또는 `assertion` 문법을 사용해야 연산이 진행된다.

  ```typescript
  //error
  function add2(x :number | string) {
    return x + 2 // x에 대한 타입이 불명확하기 때문에 연산하지 않음
  }
  
  //success
  function add2(x :number | string) {
    if (typeof x ==='number'){
      return x + 2 // x를 number로 확정하여 주었기 때문에 연산함
    }
  }
  ```

  

- object도 정의가 너무 길면 `type` 키워드로 변수에 담아 사용할 수 있다. `type` 키워드 대신 최근에 나온 `interface` 키워드를 사용해도 무방하다(차이점이 별로 없음). 특정 속성이 선택사항이라면 물음표를 기입할 수도 있다.

  ```typescript
  type objType = {
    name : string,
    age? : number
  }
  
  let rapmonster : objType {
    name : 'namjun'
  }
  ```

  

- object 안에 어떤 속성이 들어갈 지 아직 모른다면 그냥 전부 다 타입을 지정해 버릴 수도 있다. (index signature)

  ```typescript
  type objType = {
    [key :string] = number,
  }
  
  let person = {
    age : 50,
    weight : 64
  }
  ```

  

- class도 타입 설정이 가능하다. 단, 중괄호 내에 미리 변수를 만들어 놔야 constructor 안에서 사용할 수 있다.

  ```typescript
  class Person {
    name;
    constructor(name :string){
      this.name = name;
    }
  }
  ```

  