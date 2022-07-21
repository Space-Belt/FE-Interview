# 스코프란?

> 단어의 뜻에서 알 수 있듯이 범위라는 뜻을 가지며
> 프로그래밍에선 ‘식별자에(를) 접근(참조) 가능한 범위’ 라고 할 수 있다.

### \*식별자란?

```jsx
const animal = "dog";
let fruit = "apple";
var address = "LA";
function helloWorld() {}
```

위의 변수, 함수 모두를 식별자라 한다.

**고로! 변수 , 함수가 참조될 수 있는 범위 라고 보면 될것 같다.**

### 자바스크립트에서 스코프는 Global(전역) , Local(지역) 두 가지로 나뉜다.

### Global Scope(전역)

```jsx
var hello = "Hello";

console.log(hello); // => 'Hello'

function globalScope() {
  console.log(hello);
  hello = "Hi My name is Joo";
  console.log(hello);
}

globalScope(); // => 'Hello' ,'Hi My name is Joo'
console.log(hello); // => 'Hi My name is Joo'
```

위 코드에서 ‘hello’ 는 함수 밖에서 선언되었으나 ‘globalScope’ 함수 내부에서 접근했으며 ‘globalScope’함수가 실행되면 처음엔 함수 밖에서 할당된 ‘Hello’ 가 콘솔에 출력된 후 함수내에서 변수 ‘hello’가 가지는 값이 재할당 되어 출력되는것을 확인해 볼 수 있다.

이것이 가능한 이유는 함수 밖에서 선언한 **hello는 전역변수**로 선언되었기 때문이다.

```jsx
function localScope() {
  var hello = "Hi My name is Joo";
  console.log(hello);
}

localScope(); // => 'Hi My name is Joo'
console.log(hello); // => Uncaught ReferenceError ReferenceError: hello is not defined
```

반대로 ‘localScope’ 함수 안에서 선언된 변수 ‘hello’는 함수를 실행시켜보면 함수 내에서 할당된 값과 같은 변수 ‘hello’ 의 값을 콘솔로그를 확인할 수 있다.

하지만 직접 함수내에서 선언된 변수 ‘hello’ 를 콘솔로그로 확인하려 하면 참조오류가 발생하고, 변수 ‘hello’ 는 정의 되지 않았다는 문구를 볼 수 있다.

이는 함수내에서 선언된 변수는 **local Scope (지역변수)**의 하나인 **function Scope(함수 스코프)** 이기 때문이다.

### ⇒ 전역 스코프에서 선언된 변수는 지역스코프에서 접근이 가능

### ⇒ 지역 스코프내에서 선언된 변수는 전역 스코프에서 접근이 불가능

---

## 그럼 Scope 특징에 대해 알아보자! (\* {} ⇒ 이것을 블록이라한다)

\*{} ⇒ if문( `if(){}` ), 반복문 ( `for(){}` ), 객체 ( `{a = 1, b =2}` ), 함수 ( `function a(){}` )

### **블록 레벨 스코프 ( 코드블록 {} 내에서만 접근가능한 범위를 말함 )**

- 주로 C 언어 기반의 언어들이 블록 레벨 스코프를 따른다고 한다.
- 하지만 ES6에서 추가된 let 을 사용하면 블록 레벨 스코프를 사용 가능하다.
  ```jsx
  var a = 1;
  {
    var a = 5;
    console.log(a); // => 1
  }
  console.log(a); // => 1

  let z = 1;
  {
    let z = 5;
    console.log(z); // => 5
  }
  console.log(z); // => 1
  ```
  - var로 선언한 변수 a는 값이 변경되어 출력 되는것을 확인 가능하다.
  - let으로 선언된 변수 z는 블록 {} 따로 블록 {} 외부에서 선언된 변수 z 의 값과 다른것을 확인 가능하다.
  - 즉 let 을 사용하면 블록 레벨 스코프가 사용 가능한것이다.

### **함수 레벨 스코프 ( function(){} 의 {} 함수 블록 내에서만 접근 가능한 범위이다. )**

- 위의 globalScope 와 localScope함수 예제에서 봤듯 **자바스크립트는 함수 레벨 스코프를 따른다!**
- 함수레벨 스코프의 한가지 특징으로 **함수내부의 함수**(내부 함수)의 경우 상위 함수의 변수를 참조 할 수 있다!
  ```jsx
  const animal = "eagle";

  function what() {
    let animal = "dolphin";
    console.log(animal); // => 'dolphin'

    function kind() {
      // 내부함수
      animal = "tiger";
      console.log(animal);
    }
    kind(); // => 'tiger'
  }

  what(); // =>
  console.log(animal);
  ```
  - 코드를 보면 전역변수 ( `const animal = 'eagle';` ) 와 상관 없이 함수 내에서 이름이 중복되는 변수를 선언하여 사용이 가능하다는 것을 알 수있다.
  - 또한 내부 함수에선 상위 함수의 변수를 참조, 수정이 가능한것을 볼 수 있다.
  - 주의 할 점은 상위 함수에서 const로 선언되면 재할당이 불가능하다.
  - var let const의 차이점은 다음에 알아보도록 할것이다!

---

## 상위 스코프를 결정하는 방법 ( 스코프 작동 방식 )

### 렉시컬 스코프 / 동적 스코프

### 렉시컬 스코프 ( 자바스크립트 )

- 렉시컬 스코프 방식은 함수를 어디서 호출 했는지가 아니라 어디에서 선언됐는지를 따진다.
  ```jsx
  const animal = "eagle";

  function what() {
    console.log(animal);
  }

  function kind() {
    const animal = "tiger";
    what();
  }

  what(); // => 'eagle'
  kind(); // => 'eagle'
  ```
  - animal 변수는 전역에서 선언되었고 전역에 선언된것을 기준으로 삼는다.
  - 두 함수 모두 전역에서 선언됐다, what함수는 전역에 선언된 animal변수를 기준으로 하기에 ‘eagle’ 을 콘솔에 출력할것이고, kind 함수에서 호출되는 what함수도 ‘eagle’을 출력한다.

### 동적 스코프 ( 자바스크립트는 동적스코프를 사용하지 않는다 )

- 동적 스코프 방식은 변수가 함수에서 선언됐건 어디서 어떻게 선언됐는지와 무관하게, 어디서 호출됐는지만 따진다. ( 아래의 자바스크립트 코드를 예시로 알아보자 )
  ```jsx
  function dynamicScopeTest() {
    console.log(z);
  }

  function dynamicScopeMain() {
    var z = "hello";
    dynamicScopeTest();
  }

  var z = "hi";
  dynamicScopeMain(); // => 'hi'
  ```
  - 예시를 보면 dynamicScopeMain 함수 내부에서 z 가 선언되고 `var z = 'hi'` 이후에 함수dynamicScopeMain가 실행 되며, ‘hi’ 를 출력하는 것을 볼 수 있다.
  - 이것은 자바스크립트가 동적 스코프를 따르지 않기 때문이다.
  - 자바스크립트가 동적 스코프를 따랐다면 함수dynamicScopeMain를 실행시켰을때 ‘hello’를 출력했을 것이다.

---

## 전역변수를 사용할때에 주의할 점

> 전역변수의 사용은 변수 이름 중복 가능성이 있다.
> 또한 의도치 않은 재할당으로 코드를 어지럽게 하므로 사용을 지양해야한다.

### 방지하는 방법 두가지를 알아보자!

### 1. 전역 변수 객체 만들기 (공통의 전역변수용 객체 생성)

```jsx
const Animal = {};

Animal.sky = {
  name: "eagle",
  food: "meat",
};

console.log(Animal.sky.name); // => 'eagle'
```

- 위와 같이 객체를 하나 생성하고, 생성된 객체를 통해 값에 접근하는 방법이다.
- 개발 시 공통색상같은 것을 사용할때 방식과 동일하다 볼 수 있다.

### 2. 즉시실행함수(IIFE, Immediately-Invoked Function Expression)를 사용하기

```jsx
(function () {
  const Animal = {};

  Animal.sky = {
    name: "eagle",
    food: "meat",
  };

  console.log(Animal.sky.name); // => 'eagle'
})();

console.log(Animal.sky.name); // => ReferenceError ReferenceError: Animal is not defined
```

- 위와 같이 즉시실행 함수는 즉시 실행되고, 그 후 바로 사라지기에 즉시실행 함수 내에서 쓰인 변수들이 사라진다.
- 즉 실행 후 사라지기에 밖에서 사용이 불가능하다!
