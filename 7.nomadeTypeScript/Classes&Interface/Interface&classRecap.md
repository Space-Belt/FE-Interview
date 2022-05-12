# Interfaces & Type 복습

---

- 인터페이스를 사용해 클래스에 특정 메소드나 property를 상속하도록 했다.
- 인터페이스는 원하는 메소드와 property를 클래스가 가질도록 강제할수 있게 해준다.
- 인터페이스는 자바스크립트로 컴파일되지 않는다.
- 추상클래스와 비슷한 접근제어자가 가능하지만, 인터페이스는 자바스크립트파일에서 보이지 않는다.
- 추상클래스를 사용하면 자바스크립트에서는 일반적인 클래스가 된다. → 즉 파일 크기가 좀 더 커지고, 추가 클래스가 만들어진다.
- 만약 추상클래스를 다른 클래스들이 특정모양을 따르도록 하기 위한 용도로 사용한다면 같은 역할을 하는 인터페이스를 사용하는것이 좋다
- `class Player extends User` 처럼 쓰는법 대신 `class Player implements User` 로 써줘야 한다.
- 타입스크립트가 여전히 보호해주지만, 인터페이스가 자바스크립트 코드에서 보이지 않게 된다.

## 타입과 인터페이스를 비교해보자!

## Type

- type을 쓰고싶다면 type 키워드를 사용하면 된다.
- 이때 여러가지 옵션들이 있다.
  1. 오브젝트의 모양을 설명하는것이다.
  2. 타입 alias를 만드는것
  3. 타입을 특정된 값으로 만드는 것이다.

---

1. 을 비교해보자.

```jsx
// 타입
type PlayerA = {
  name: string,
};

type PlayerAA = PlayerA & {
  lastName: string,
};

// 1번
//const player: PlayerA = {
//    name:"woo"
//}

const player: PlayerAA = {
  name: "woo",
  lastName: "joo",
};

// 인터페이스
interface PlayerB {
  name: string;
}

interface PlayerBB extends PlayerB {
  lastName: string;
}

// 2번
//const playerB: PlayerB = {
//    name:"woo"
//}

interface PlayerBB extends PlayerB {
  lastName: string;
}

const playerB: PlayerBB = {
  name: "woo",
  lastName: "joo",
  health: 3,
};
```

- 1, 2 번 둘 다 오브젝트의 모양과 타입을 잘 알려주고 있다. 하지만 둘을 봤을때 뭐가 인터페이스를 사용한것인지는 알 수 없다.
- 하지만 두가지가 할 수 있는 것들이 다르다!
- 타입 !
  - 타입을 상속하려면 또 다른 타입 하나를 만들어 PlayerAA타입이 PlayerA 타입과 lastName을 가지는 오브젝트를 합친 거라고 알려줘야 한다. & 연산자
  - 프로퍼티를 따로 또 생성하려면 다른 이름으로 새 타입을 만든 후 & 연산자 사용하며 남은 것들을 써줘야 한다.
- 인터페이스
  - 인터페이스를 상속하려면 extends ( 객체지향 프로그래밍의 컨셉과 매우 유사 )
  - 프로퍼티를 계속 여러개 생성 가능하다.

---

### 인터페이스와 타입의 상속과 추상클래스 대체

```jsx
type PlayerA = {
    firstName:string
}

class UserA implements PlayerA {
    constructor(
        public firstName:string
    ){}
}

interface PlayerB {
    firstName:string
}

class UserB implements PlayerB {
    constructor(
        public firstName:string
    ){}
}
```

- 이렇게 둘다 상속이 가능하다.
- 또한 둘 모두 추상 클래스를 대체할 수 있다.
- 타입스크립트 커뮤니티에서는 클래스나 오브젝트의 모양을 정의하고 싶다면 인터페이스르 사용하고, 다른 모든 경우에는 타입을 사용하라고 하고 있다.
- 타입스크립트를 생성해주는 큰 프로젝트를 하면, 대부분 인터페이스를 만들어 준다. → 그 이유는 인터페이스를 상속시키는 방법이 직관적이어서다. ( 그냥 적기만 하면 수많은 인터페이스 정의를 합칠 수 있다. )
- 타입 alias 나, 특정 값으로 타입을 제한하는 경우와 같은 다른 경우에는 타입을 사용 가능하다.
- 타입스크립트 공식문서에서 확인해보면 대부분의 경우에 타입과 인터페이스가 매우 유사하기에 둘 중 하나를 자유롭게 선택가능하다고 나와있다. 또한 인터페이스의 대부분의 기능은 타입에도 있다고 한다.
- 가장 큰 차이 점은 타입은 새 property 를 추가하기 위해 다시 선언될 수 없지만 인터페이스는 항상 상속이 가능하다.

    <img width="546" alt="image" src="https://user-images.githubusercontent.com/82592845/168007379-281fb1bb-bccc-4154-9c1f-b64721b17784.png">
    
    <img width="569" alt="image" src="https://user-images.githubusercontent.com/82592845/168007603-aee559ad-edc9-4e20-8af0-f4c8f341e803.png">
    
    - 위의 두 사진이 가장 큰 차이점이다!

- 클래스는 인터페이스와 타입 둘다 상속이 가능하다. 또한 추상클래스는 타입과 인터페이스로 대체 가능하다.

### 결론

- 타입스크립트에게 오브젝트의 모양을 알려주기 위해선 인터페이스를 사용해라!
- 나머지 상황에서는 타입을 사용해라!
