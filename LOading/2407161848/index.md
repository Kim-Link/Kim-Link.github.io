---
layout: post
type: LOading
date: 2024-07-16 18:48
category: 개발
title: OOP 재 적립
subtitle: 다시 한번 OOP... 근데 한번 더 적을듯...
writer: 0
post-header: true
header-img: ../OOP.png
hash-tag:
  - OOP
  - 술
---


# Intro
이쯤 되니 다시 한번 OOP에 대해서 짚고 넘어가야 할거 같다.

이 글로 한번 정리 하고 나서 조영호 저자의 `오브젝트`라는 책을 한번 읽어 보고, 한번더 글을 써볼까 한다.

oop를 살펴 보기 전에 먼저 클래스와 객체에 대해서 먼저 알아보자.

<br>

# 객체, 인스턴스, 클래스

<br>

## 객체

- **객체**는 인스턴스를 추상화 하여 나타낸것이다.
- 객체는 클래스에서 정의한 속성과 메서드를 가지고 있다.
- 프로그램 내에서 조작할 수 있는 개체를 의미

<br>

## 인스턴스 (Instance)

- **정의**: 인스턴스는 특정 클래스의 객체를 의미한다. 즉, 클래스를 기반으로 생성된 개별 객체를 말한다. "인스턴스화"는 클래스를 사용하여 새로운 객체를 생성하는 과정을 의미한다.
- 실제 사례를 뜻한다.
- **예시**: `Dog` 클래스에서 `dog1`과 `dog2`라는 두 개의 인스턴스를 생성할 수 있다. 여기서 `dog1`과 `dog2`는 각각 `Dog` 클래스의 인스턴스다.

<br>

## 클래스 (Class)
- 클래스는 객체를 생성하기 위한 청사진 또는 템플릿이다.
- 클래스는 속성(데이터)과 메서드(함수)를 정의한다.

> **속성(Attributes)**: 객체의 상태를 나타내는 변수. 클래스에서 정의된 속성들이 객체에 실제로 저장된다.
> **메서드(Methods)**: 객체가 수행할 수 있는 동작을 정의하는 함수. 클래스에서 정의된 메서드들이 객체에 의해 호출될 수 있습니다.

- 클래스는 특정 유형의 객체를 만드는 데 필요한 모든 것을 포함한다.

<br>

### 예시
```ts
class Animal {
  // 공개 속성
  public name: string;

  // 보호된 속성
  protected age: number;

  // 비공개 속성
  private secret: string;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
    this.secret = 'secret';
  }

  // 공개 메서드
  public speak(): string {
    return `${this.name} makes a sound.`;
  }

  // 보호된 메서드
  protected getAge(): number {
    return this.age;
  }

  // 비공개 메서드
  private getSecret(): string {
    return this.secret;
  }
}
```


- **공개 속성** (`public`):
    - `name` 속성은 `public`으로 선언되어 클래스 외부에서 접근할 수 있다.

- **보호된 속성** (`protected`):
    - `age` 속성은 `protected`로 선언되어 해당 클래스와 **서브 클래스**에서 접근할 수 있다.
    - **서브 클래스**(또는 하위 클래스)는 상위 클래스(또는 슈퍼 클래스)를 상속받아 그 특성과 행동을 물려받는 클래스 이다.

- **비공개 속성** (`private`):
    - `secret` 속성은 `private`로 선언되어 해당 클래스 내에서만 접근할 수 있다.

- **메서드**:
    - `getAge`와 `setAge` 메서드는 `protected`로 선언된 `age` 속성에 접근하고, `getSecret`와 `setSecret` 메서드는 `private`로 선언된 `secret` 속성에 접근한다.

- **생성자**(`constructor`):
    - `constructor`는 클래스의 인스턴스가 생성될 때 호출되는 메서드 이다.
    - 객체의 **초기 상태를 설정**하는 역할을 하며, 주로 객체의 속성을 초기화하는 데 사용된다.
    - 다양한 프로그래밍 언어에서 생성자의 역할은 비슷하지만, 그 구현 방식에는 약간의 차이가 있다.
        - python에서는 `__init__(self,name,age)`로 표현한다.
    - TypeScript에서는 `constructor` 메서드를 사용하여 클래스를 초기화할 수 있다. 생성자는 클래스의 인스턴스가 생성될 때 자동으로 호출된다.
    - typescript에서 구문을 줄여 쓸수 있다.

```ts
class Animal {
  public name: string;

  constructor(name: string) {
    this.name = name;
  }
}
```


```ts
class Animal {
  constructor(public name: string) {}
}
```

<br>

## 클래스와 객체, 인스턴스와 차이점

- **객체**: 형식, 하나의 단위 이다.(추상적)
- **인스턴스** : 특정 클래스에 대한 사례(인스턴스)이다.
- **클래스**: 객체를 기술하는 언어적인 문법이다.

<br>

# 객체 지향 프로그래밍(OOP)

객체지향은 결국 코드를 나눠서 이해하기 쉽고 유지 관리가 쉽게 하기 위한 노력이다.
객체 지향의 핵심은 객체나 클래스가 아니라 `메세지` 다.

클래스의 구조와 메서드가 아니라 객체의 역할, 책임, 협력에 집중해야한다.
객체지향은 객체를 지향하는 것이지 클래스를 지향하는 것이 아니다.

<br>

## 협력(collaboration)
요청과 응답으로 협력한다.
협력은 한 사람이 다른 사람에게 도움을 요청할때 시작된다.
자신에게 할당된 일이나 업무를 처리하던 중에 스스로 해결하기 어려운 문제에 부딪히게 되면 문제를 해결하는데 필요한 지식을 알고 있거나 도움을 받을수 있는 누군하에게 요청하게 된다.
요청 받은 사람은 일을 처리한 후 요청한 사람에게 필요한 지식이나 서비스를 제공하는 것으로 요청에 응답한다.

<br>

## 책임(responsibility)
객체지향에서 어떤 객체가 어떤 요청에 대해 대답해 줄수 있거나 적절한 행동을 할 의무가 있는 경우 해당 객체가 책임을 가진다고 말한다.
책임은 객체 지향 설계의 가장 중요한 재료다.
"객체지행 개발에서 가장 중요한 능력은 책임을 능숙하게 소프트웨어 객체에 할당하는 것"
→ 크레이그 라만(Claig Larman)
책임을 어떻게 구현할 것인가 하는 문제는 객체와 책임이 제자리를 잡은 후에 고려해도 늦지 않다.
책임은 '하는것(doing)'과 '아는것(knowing)'으로 구성된다.

<br>

## 역할(role)
역할은 협력 내에서 다른 객체로 대체할수 있음을 나타내는 일종의 표식이다. 협력 안에서 역할은 "이 자리는 해당 역할을 수행할 수 있는 어떤 객체라도 대신할 수 있습니다."라고 말하는 것과 같다.

역할을 대체하기 위해서는 각 역할이 수신할 수 있는 메시지를 동일한 방식으로 이해해야 한다.
즉, 역할을 대체할 수 있는 객체는 동일한 메시지를 이해 할수 있는 개체로 한정된다.
메시지가 책임을 의미한다.

<br>

## 객체지향의 본질
- 객체지향이란 시스템을 상호작용하는 자율적인 객체들의 공동체로 바라보고 객체를 이용해 시스템을 분할하는 방법이다.
- 자율적인 객체란 상태와 행위를 함께 지니며 스스로 자기 자신을 책임지는 객체를 의미한다.
- 객체는 시스템의 행위를 구현하기 위해 다른 객체와 협력한다. 각 객체는 협력 내에서 정해진 역활을 수행하며 역활은 관련된 책임의 집합이다.
- 객체는 다른 객체와 협력하기 위해 메시지를 전송하고, 메시지를 수신한 객체는 메시지를 처리하는데 적합한 메서드를 자율적으로 선택한다.

<br>

## 캡슐화(Encapsulation)
- 데이터와 메서드를 하나의 단위로 묶어 외부로부터 보호한다.
- 캡슐화는 클래스의 내부 상태를 숨기고, 외부에서 접근할 수 있는 인터페이스만 제공하여 객체의 무결성을 유지한다.
- 이를 통해 코드의 복잡성을 줄이고, 유지보수를 용이하게 한다.
- 접근 제어자 (`public`, `private`, `protected`)를 통해 클래스의 멤버에 대한 접근 수준을 정의한다.
- **데이터 은닉 (Data Hiding)**: 데이터를 보호하고, 잘못된 사용으로부터 객체를 안전하게 한다.

<br>

### getter 메서드와 setter 메서드
- OOP에서 클래스의 속성에 접근하고 수정하는 방법을 제공하는 특별한 메서드
- 속성 접근을 제어하고 데이터의 무결성을 유지하는 데 중요한 역할
- TypeScript에서는 getter와 setter를 속성처럼 사용하여 직관적이고 간단하게 속성에 접근할 수 있다.
- `get`, `set` 사용: 접근자 메서드(**getter**와 **setter**라고 부른다.)

```ts
class Person {
  private _name: string;
  private _age: number;

  constructor(name: string, age: number) {
    this._name = name;
    this._age = age;
  }

  // getter 메서드
  public get name(): string {
    return this._name;
  }

  public get age(): number {
    return this._age;
  }

  // setter 메서드
  public set name(newName: string) {
    if (newName && newName.length > 0) {
      this._name = newName;
    } else {
      throw new Error("Invalid name");
    }
  }

  public set age(newAge: number) {
    if (newAge > 0) {
      this._age = newAge;
    } else {
      throw new Error("Invalid age");
    }
  }

  public introduce(): void {
    console.log(`Hi, my name is ${this._name} and I am ${this._age} years old.`);
  }
}

const person = new Person("John", 30);
console.log(person.name); // 출력: John
person.name = "Jane"; // setter 호출
console.log(person.name); // 출력: Jane

console.log(person.age);  // 출력: 30
person.age = 25;          // setter 호출
console.log(person.age);  // 출력: 25

person.introduce(); // 출력: Hi, my name is Jane and I am 25 years old.
```

<br>

## 상속(Inheritance)
: 기존 클래스를 바탕으로 새로운 클래스를 만들 수 있다. 코드 재사용성을 높인다.
- **부모 클래스(Superclass)**: 다른 클래스가 상속받는 클래스이다. 기본 기능과 속성을 정의한다.
- **자식 클래스(Subclass)**: 부모 클래스를 상속받는 클래스이다. 부모 클래스의 속성과 메서드를 물려받으며, 필요에 따라 새로운 속성과 메서드를 추가하거나 부모 클래스의 메서드를 재정의(오버라이딩)할 수 있다.

```ts
// 부모 클래스
class Animal {
  name: string;
  
  constructor(name: string) {
    this.name = name;
  }
  
  makeSound(): void {
    console.log(`${this.name} makes a sound.`);
  }
}

// 자식 클래스
class Dog extends Animal {
  breed: string;

  constructor(name: string, breed: string) {
    super(name); // 부모 클래스의 생성자를 호출합니다.
    this.breed = breed;
  }

  makeSound(): void { // 메서드 오버라이딩
    console.log(`${this.name} barks.`);
  }

  fetch(): void {
    console.log(`${this.name} is fetching.`);
  }
}

//사용
const animal = new Animal("Generic Animal");
animal.makeSound(); // 출력: Generic Animal makes a sound.

const dog = new Dog("Buddy", "Golden Retriever");
dog.makeSound(); // 출력: Buddy barks.
dog.fetch(); // 출력: Buddy is fetching.
```

<br>

### 상속의 장점

- **코드 재사용**: 공통 기능을 부모 클래스에 정의하여 자식 클래스에서 재사용할 수 있다.
- **계층 구조**: 객체 간의 계층 구조를 통해 시스템을 더 체계적으로 설계할 수 있다.
- **유연성**: 자식 클래스가 부모 클래스의 기능을 확장하거나 수정할 수 있다.

<br>

### 요약
상속은 객체 지향 프로그래밍의 핵심 개념 중 하나로, 기존 클래스의 속성과 메서드를 새로운 클래스에서 재사용하고 확장할 수 있게 한다. 이를 통해 코드의 재사용성을 높이고, 유지보수를 쉽게 할 수 있다. TypeScript에서는 `extends` 키워드를 사용하여 상속을 구현하며, `super` 키워드를 사용하여 부모 클래스의 생성자와 메서드를 호출할 수 있다.

<br>

## 다형성(Polymorphism)
: 동일한 메서드가 다른 객체에서 다르게 동작할 수 있다.
객체 지향 프로그래밍(OOP)에서 다형성(Polymorphism)은 같은 인터페이스나 부모 클래스를 공유하는 여러 객체가 각기 다른 방식으로 동작할 수 있게 하는 특성이다.

<br>

### 다형성의 주요 개념

1. **오버라이딩 (Overriding)**: 자식 클래스가 부모 클래스의 메서드를 재정의하여 다른 동작을 수행하는 것.
2. **오버로딩 (Overloading)**: 같은 이름의 메서드를 매개변수의 타입이나 개수에 따라 다르게 정의하는 것.
3. **인터페이스와 추상 클래스**: 공통 인터페이스나 추상 클래스를 통해 서로 다른 클래스가 동일한 메서드를 구현하도록 강제하는 것.

```ts
// 부모 클래스 (Animal)
abstract class Animal {
  constructor(public name: string) {}

  // 추상 메서드 (구현 없음)
  abstract makeSound(): void;

  // 일반 메서드
  move(): void {
    console.log(`${this.name} is moving.`);
  }
}


// 자식 클래스 (Dog)
class Dog extends Animal {
  constructor(name: string) {
    super(name);
  }

  // 추상 메서드 구현
  makeSound(): void {
    console.log(`${this.name} barks.`);
  }
}

// 자식 클래스 (Cat)
class Cat extends Animal {
  constructor(name: string) {
    super(name);
  }

  // 추상 메서드 구현
  makeSound(): void {
    console.log(`${this.name} meows.`);
  }
}


// 사용
function animalSound(animal: Animal) {
  animal.makeSound();
}

const dog = new Dog("Buddy");
const cat = new Cat("Whiskers");

animalSound(dog); // 출력: Buddy barks.
animalSound(cat); // 출력: Whiskers meows.
```

<br>

### 다형성의 이점

1. **코드 재사용성**: 부모 클래스나 인터페이스를 사용하여 공통된 기능을 정의하고, 자식 클래스나 구현 클래스를 통해 이를 재사용할 수 있다.
2. **유연성**: 같은 인터페이스나 부모 클래스를 공유하는 객체들은 서로 교환 가능하며, 런타임에 객체의 타입을 변경할 수 있다.
3. **유지보수성**: 새로운 클래스나 기능을 추가할 때 기존 코드를 수정하지 않고도 쉽게 확장할 수 있다.
4. **가독성**: 일관된 인터페이스를 사용하여 코드를 작성함으로써, 코드의 가독성과 이해도를 높일 수 있다.

<br>

### 오버라이딩과 오버로딩
- **오버라이딩**: 자식 클래스에서 부모 클래스의 메서드를 재정의하는 것.
- **오버로딩**: 같은 이름의 메서드를 여러 버전으로 정의하는 것

<br>

## 추상화(Abstraction)
: 불필요한 세부 사항을 숨기고 중요한 부분에 집중할 수 있게 합니다.

추상화는 객체 지향 프로그래밍(OOP)의 중요한 개념 중 하나로, 복잡한 시스템을 더 단순하고 이해하기 쉽게 만드는 과정이다. 추상화는 객체의 중요한 특징을 강조하고, 불필요한 세부 사항을 숨겨 복잡성을 줄이는 것을 목표로 한다. 이를 통해 시스템의 주요 개념과 인터페이스를 명확히 정의할 수 있다.

<br>

### 추상화의 주요 목적

1. **복잡성 감소**: 시스템의 복잡한 내부 구조를 숨기고, 외부에 필요한 부분만을 드러내어 사용자가 더 쉽게 시스템을 이해하고 사용할 수 있게 한다.
2. **유연성 및 재사용성 증가**: 추상화를 통해 만들어진 인터페이스나 클래스는 다른 프로그램에서도 쉽게 재사용될 수 있으며, 변경이 용이하다.
3. **모듈화**: 시스템을 더 작은 모듈로 나누어 관리하기 쉽게 만든다.

<br>

### 추상화의 예

<br>

#### 1. 클래스와 인터페이스

```ts
interface Shape {
    area(): number;
    perimeter(): number;
}

class Circle implements Shape {
    radius: number;

    constructor(radius: number) {
        this.radius = radius;
    }

    area(): number {
        return Math.PI * this.radius * this.radius;
    }

    perimeter(): number {
        return 2 * Math.PI * this.radius;
    }
}

class Rectangle implements Shape {
    width: number;
    height: number;

    constructor(width: number, height: number) {
        this.width = width;
        this.height = height;
    }

    area(): number {
        return this.width * this.height;
    }

    perimeter(): number {
        return 2 * (this.width + this.height);
    }
}
```

위 예제에서 `Shape` 인터페이스는 도형의 면적과 둘레를 계산하는 메소드들을 정의한다. `Circle`과 `Rectangle` 클래스는 `Shape` 인터페이스를 구현하여 구체적인 도형의 면적과 둘레를 계산하는 방법을 제공한다.

<br>

#### 2. 추상 클래스

추상 클래스는 하나 이상의 추상 메소드를 포함할 수 있는 클래스입니다. 추상 클래스 자체는 인스턴스화될 수 없으며, 이를 상속하는 클래스가 추상 메소드를 구현해야 합니다.

```ts
abstract class Animal {
    abstract makeSound(): void;

    move(): void {
        console.log('Moving...');
    }
}

class Dog extends Animal {
    makeSound(): void {
        console.log('Woof! Woof!');
    }
}

class Cat extends Animal {
    makeSound(): void {
        console.log('Meow! Meow!');
    }
}

let dog = new Dog();
dog.makeSound(); // 출력: Woof! Woof!
dog.move(); // 출력: Moving...

let cat = new Cat();
cat.makeSound(); // 출력: Meow! Meow!
cat.move(); // 출력: Moving...

```

위 예제에서 `Animal` 클래스는 추상 클래스이며, `makeSound`라는 추상 메소드를 가지고 있다. 

`Dog`와 `Cat` 클래스는 `Animal` 클래스를 상속받아 `makeSound` 메소드를 구현한다.

추상화는 객체 지향 프로그래밍의 기본 원리 중 하나로, 복잡한 시스템을 관리하고 유지보수하는 데 필수적인 개념이다. 

이를 통해 시스템의 복잡성을 줄이고, 코드의 재사용성과 유지보수성을 높일 수 있다.

<br>

### 정리
- 추상화는 input 과 output에 집중하며 그 사이 관계에 대해서는 신경쓰지 않는다.
- 인터페이스를 통해 무엇인지 유추할수 있게 한다.
- 추상 클래스는 그 자체 인스턴스를 만들수 없다. 자식 클래스를 통해서만 인스턴스를 생성한다.



객체지향의 핵심을 클래스를 어떻게 구현할 것인가가 아니라 객체가 협력 안에서 어떤 책임감과 역활을 수행할 것인지를 결정하는 것이다.

<br>

## 참고문헌

- [객체지향의 사실과 오해](https://product.kyobobook.co.kr/detail/S000001628109?utm_source=google&utm_medium=cpc&utm_campaign=googleSearch&gt_network=g&gt_keyword=&gt_target_id=aud-901091942354:dsa-435935280379&gt_campaign_id=9979905549&gt_adgroup_id=132556570510&gad_source=1&gclid=CjwKCAjwl6-3BhBWEiwApN6_ksTr7Scws0pWmZJ8dThss9dMmoS4CliyJnTgXTwcYx8u6dv1W5LPGhoCWfcQAvD_BwE)
