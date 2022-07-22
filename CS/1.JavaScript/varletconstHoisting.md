# var, let, const 호이스팅

---

## 먼저 변수란 무엇인가에 대해 알아보자.

> 값을 저장하기 위해 확보한 메모리 공간 자체 또는 메모리 공간을 식별하기 위한 식별자이다.

변수를 생성하는 것은 변수의 선언이라고 한다.

풀어서 설명하자면, 값을 저장하기 위한 메모리 공간을 확보하며, 변수 이름과 확보된 메모리 공간의 주소를 연결해 값을 저장가능하도록 준비하는이다.

### 자바스크립트의 변수 생성을 알아보자!

```jsx
let animal; // 변수 선언
animal; // 변수에 값 할당

let fish = "shark"; // 변수 선언과 할당을 동시에

let insect, bug; // , 콤마를 사용하여 두가지 변수를 선언
let insect = "butterFly",
  bug = "cockroach"; // , 사용 => 두가지 변수 값 할당
```

- 위와 같이 값을 할당하지 않고 변수만 선언 한것이 `let animal;`
- 변수 선언과 할당을 동시에 한것이 `let fish = 'shark'`
- 그 외에 콤마 ( , ) 를 사용하여 여러가지 변수를 동시에 할당 할 수 있다!
- 변수는 선언, 초기화, 할당 단계를 거친다.
  - **선언단계 ⇒** 변수가 변수 객체에 등록된다. 이 변수 객체는 스코프가 참조하는 대상이 된다.
  - **초기화 단계** ⇒ 변수 객체에 등록된 변수의 공간을 메모리에 확보한다. 그리고 변수는 undefined로 초기화 된다.
  - **할당 단계** ⇒ undefined로 초기화된 변수에 실제의 값을 할당한다.

## var, let, const ????

### var

> 먼저 var는 ES6 에서 let과 const가 나오기 전에 모든 선언을 담당했던 키워드였다. 그런데 왜 let과 const가 나왔을까??

### var에는 문제점이 있었기 때문이다. 그럼 그 문제에 대해 살펴보자!

1. **var 키워드로 선언한 변수는 지역 스코프에서 선언되어도 모든 스코프에서 접근이 가능하다.
   반복문에서 정의된 변수가 반복문이 끝난 이후에도 계속 있다는 것이다.**

```jsx
for (var i = 0; i < 5; i++) {
  console.log(i);
} // => 0 1 2 3 4

console.log(i); //=> 5   반복문의 변수에 접근
```

var는 오로지 ‘함수레벨 스코프'이기 때문이다. ( ⇒ 함수의 코드 블록 범위 까지만을 스코프로 인정 ) ( 아래의 코드에서 확인 해보자 )

이러한 var변수의 스코프를 제한하기 위한 방법으로는 즉시 실행 함수를 사용하는 방법이 있다.

```jsx
function test() {
  for (var i = 0; i < 5; i++) {
    continue;
  }
  console.log(i); //=> 5
}

console.log(i); // 참조오류 : i는 정의되지 않았습니다.
```

var는 오로지 ‘함수레벨 스코프' 이것을 기억하자!

1. **재선언, 재할당이 가능하다.**

   ```jsx
   var animal = "dog";
   console.log(animal); // => dog
   var animal = "cat";
   console.log(animal); // => cat
   ```

   - 이렇게 바로 재선언이 되며 아무 오류도 출력하지 않는다.
   - 코드가 길어지게 되어 변수 명을 재사용하게 되면 오류가 발생하게 되기에 좋지 않다.

2. 변수 호이스팅이 이루어진다.

   ```jsx
   console.log(animal); // => undefined;
   console.log(bird); // => ReferenceError: Cannot access 'bird' before initialization
   console.log(ant); // => ReferenceError: Cannot access 'bird' before initialization

   var fish = "shark";
   var animal = "dog";

   let bird = "eagle";
   const insect = "ant";
   ```

   - 이렇게 변수의 선언이 되기 전에 console.log를 찍어도 에러가 나오지 않고 값이 나온다.
   - let과 const로 선언한 bird와 insect는 초기화전엔 접근이 불가능하다 라는 에러가 나온다.

   ```jsx
   console.log(animail);  // => undefined
   var animal = 'dog';

   -----------------------------------------------------------------

   var animal;  // var animal = undefined; 한것과 같음.
   console.log(animal); // => undefined;
   animal = 'dog';

   // var로 선언한 변수는 호이스팅이 된 후에 undefined가 할당된다. 실제 값은 상단에서
   // 정의했던 그 위치에서 할당이 되기에 undefined를 출력한다.

   -----------------------------------------------------------------

   console.log(animal); //=> undefined
   animal = 'dog';
   console.log(animal); //=> dog
   var animal = 'cat';

   // 이렇게 문제 많아보이는 코드도 에러없이 실행이 된다.
   ```

   - 밑에서 호이스팅에 대해 조금 더 알아보자!

3. var 변수를 함수에서 키워드 없이 값을 할당하면 전역 변수가 된다.

   ```jsx
   function test1() {
     i = 1;
   }

   function test2() {
     console.log(i);
   }

   test2(); // => 1
   ```

   - 다른 함수인데 전역변수가 되어 접근이 가능하다.
   - 이것을 피하기 위해서 파일의 최상단에 **‘use strict’** 를 선언할 수 있다.

## 호이스팅을 먼저 알아보고 넘어가자!

> “ 스코프 안에 있는 모든 선언들을 적용된 스코프의 최상위로 끌어올리는 것”

변수 생성과정을 보면서 var , let , const 의 호이스팅을 알아보자.

- **선언단계 ⇒** 변수가 변수 객체에 등록된다. 이 변수 객체는 스코프가 참조하는 대상이 된다.
- **초기화 단계** ⇒ 변수 객체에 등록된 변수의 공간을 메모리에 확보한다. 그리고 변수는 undefined로 초기화 된다.
- **할당 단계** ⇒ undefined로 초기화된 변수에 실제의 값을 할당한다.

### var는 선언과 동시에 초기화가 이루어 진다.

```jsx
console.log(animal); // => undefined
var animal = "dog";
```

- 참조에러가 나오지 않고 undefined 라는 값이 나온다.
- 아래의 코드가 자바스크립트가 위 코드를 호이스팅하면서 코드를 해석한 방식이다.

```jsx
var animal;
console.log(animal); // => undefined;
animal = "dog";
```

- 이렇게 호이스팅은 선언을 최상위로 올린다.
- 즉 선언과 동시에 초기화가 됐기에 undefined 란 값을 가진다.

### let과 const는 선언만 이루어 진다.

**1 )**

```jsx
console.log(animal);//=>RefError:Cannot access 'animal' before initialization
let animal = 'dog' || const animal = 'dog'
```

- 초기화 전에는 접근할 수 없다는 오류가 뜬다. 선언만 되고 초기와 되지 않았다는 것이다!

**2 )**

```jsx
animal = 'dog'//=>RefError:Cannot access 'animal' before initialization
let animal = 'cat' || const animal = 'dog'
```

- 위의 코드도 같은 에러가 발생한다. let이 호이스팅 되었다는 이유다!!
- 호이스팅 되지 않았다면 `let animal = ‘cat’` 에 `has already already declared` 오류가 발생했을것이다!!

**3 )**

```jsx
function animal() {
	return name;
}
let name = 'dog';  ||  const animal = 'dog'
console.log(animal()) //=> dog;
```

- 이렇게 dog가 잘 출력되는 것을 볼 수 있다.
- let도 호이스팅이 되지만 선언과 동시에 초기화가 되지 않기에 1 ) 에서 초기화 되지 않았다는 오류가 뜨는 것이다!

### ⇒ let과 const가 호이스팅은 되지만 선언만 된 상태로 호이스팅 되기 때문에 원래 자기 자리에서 자바스크립트에게 읽혀 초기화, 할당이 되기 전에는 ReferenceError를 발생시킨다.

![선할초](https://user-images.githubusercontent.com/82592845/180397615-d7eb583d-cd5b-408b-8ab7-511aee467e00.png)

### 이렇게 선언 후에 초기화와 할당 전까지의 사각지대에 있을때 ReferenceError가 발생한다. 이 사각지대를 Temporal Dead Zone⇒ TDZ 라 한다.

## 이제 다시 let 과 const 를 알아보자!

> ES6에서 부터 나왔다.
> let, const 는 블록레벨 스코프!

### let의 특징 !

- 키워드 생략이 불가능 ( let )
- 중복선언 불가능
- 재할당이 가능하다.
  ```jsx
  let animal = "eagle";
  animal = "dog";
  console.log(animal); // => dog
  ```

### const의 특징 !

- const는 선언과 초기화가 동시에 이루어져야 한다. ( 할당도 같이 해줘야 한다. )
- const 는 재선언 / 재할당이 불가능하다
  ```jsx
  const animal = ["dog"];
  animal.push("cat");

  console.log(animal);
  ```
  - 하지만 이런건 된다!!!

### let 대신 const를 사용하는것이 좋다.
