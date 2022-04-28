# TypeScript Literal Types?

```jsx
const userName1 = "Bob";
let userName2 = "Tom";
```

- useName1은 변하지 않는 const로 선언했기 때문에 상관 x
  - 이렇게 정해진 스트링 값을 가진것을 문자열 literal type이라 한다.
  - 앞서 enum을 배웠는데 타입으로 비슷한 형태를 만들수 있다.
  ```jsx
  type Job = "polic" | "developer" | "teacher";

  interface User {
    name: string;
    job: Job;
  }

  const user: User = {
    name: "Bob",
    //job : "student"  //오류  job에 존재하지 않음
    job: "developer",
  };

  // 이렇게 숫자형 literal 타입도 사용가능 하다.
  interface MiddleSchoolStudent {
    name: string;
    grade: 1 | 2 | 3; // | 는 union type
  }
  ```
- userName2 의 타입은 스트링으로 정의됐다. 마우스를 올려보면 `let userName2: string` 이렇게 표시되어 있다.
  - 그러므로 `userName2 = 3;` 이렇게 숫자를 재할당해줄수 없다.
  - 만약 숫자를 할당하고 싶다면 `let userName2: string | number = "Tom";` 이렇게 명시적으로 작성을 해줘야한다.

### `|` 유니온타입에대해 알아보자!

```jsx
interface Car {
  name: "car";
  color: string;
  start(): void;
}

interface Mobile {
  name: "mobile";
  color: string;
  call(): void;
}

function getGift(gift: Car | Mobile) {
  console.log(gift.color);
  //gift.start(); // err 발생 car에만 start를 가지고 있기 때문에 color는 문제없다.
  // 그러므로 start사용전에 car인지 mobile인지 확인해줘야한다.
  if (gift.name === "car") {
    gift.start();
  } else {
    gift.call();
  }
}
```

- 여기서는 if문으로 했지만 검사할 것이 만으면 switch 구문을 사용하는것이 좋다.
- if문의 gift에 마우스를 올려보면 car라고 알고 있다. 밑은 moblie !
  - 이렇게 동일한 속성의 타입을 다르게해서 구별할 수 잇는것을 식별가능한 유니언타입이라 한다.

### Intersection Types 교차 타입!

- 여러타입을 합쳐서 사용한다.
- union ( | ) 이 Or 의미였다면
- intersection은 & And를 의미한다.

```jsx
interface Car {
  name: string;
  start(): void;
}

interface Toy {
  name: string;
  color: string;
  price: number;
}

const toyCar: Toy & Car = {
  name: "타타",
  start() {},
  color: "blue",
  price: 1000,
};
```

- 이런식으로 교차타입은 여러개의 타입을 하나로 합쳐주는 역할을 한다. 즉 필요한 모든 기능을 가진 하나의 타입이 만들어지는것이다!
