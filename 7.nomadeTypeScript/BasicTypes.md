### 기본 타입들 !

```jsx
const player: {
  name: string,
  age?: number,
} = {
  name: "woo",
};

const player: {
  name: string,
  age?: number,
} = {
  name: "lynn",
  age: 15,
};
```

```jsx
type Player = {
  name: string,
  age?: number,
};

const woo: Player = {
  name: "woo",
};

const lynn: Player = {
  name: "hoo",
  age: 15,
};
```

- 알리아스를 사용 (Alias)하여 재사용성을 높였다.

```jsx
type Age = number;

type Player = {
  name: string,
  age?: Age,
};

const woo: Player = {
  name: "woo",
};

const hoo: Player = {
  name: "hoo",
};
```

- 알리아스를 이용하여 이렇게까지도 할 수 있지만 쓸대없이 하지는 말자.

### 함수의 return 타입 정해주기

```jsx
type Age = number;
type Player = {
  name: string,
  age?: Age,
};

function playerMaker(name: string, age: number): Player {
  return {
    name,
    age,
  };
}

const woo = playerMaker("woo", 15);

const playerMaker2 = (name: string): Player => ({ name });

const joo = playerMaker2("joo");
```

### 읽기 전용 ( 수정 불가 ) → readonly

```jsx
const names: readonly string[] = ["1", "2"]
```

- immutable 불변성 유지
- object 관련 끝!

### Tuple →

- array생성 , 최소한의 길이를 가지고 특정위치에 특정 타입이 있어야한다.

```jsx
const champion: [string, number, boolean] = ["woo",2,true];

const champion: readonly [string, number, boolean] = ["woo",2,true];
```

- 최소 갯수 3개는 맞춰줘야한다.
- 0,1,2 순서대로 타입이 같아야한다.

### Optional Type (age?:number)

### any ( 아무타입이나 다 가능하다 )

- 하지만 사용하지 않는 것이 좋다. 타입스크립트에서 나오는것이기 때문이다.

```jsx
const a: any[] = [1, "3", true, [1, 3, 4]];
const b: any = true;
a + b;
```

- 이렇게 해도 오류가 발생하지 않는다. ( 즉 , 좋지 않음 )
- 보호장치를 풀어주는것이다.

### void ( return이 없는 함수 ) 비어있는것을 의미한다.

```jsx
function hi(): void {
  console.log("x");
}
```

- 리턴이 없으면 굳이 void를 해주지 않아도 알아서 인식한다.

### never

```jsx
function hi(): never {
  throw new Error("xxx");
}

function hello(name: string | number): never {
  if (typeof name === "string") {
    name;
  } else if (typeof name === "number") {
    name;
  } else {
    // 여기의 타입은 never 즉 이 코드는 작동하지 않는다.
    name;
  }
}
```

- 위의 예시는 리턴하지 않고 오류를 발생시키는 함수다.
- 함수가 절대로 리턴하지 않을때 사용한다.
- 함수에서 exception(예외)가 발생할때
- 자주 사용하진 않지만 뭔지는 알자!
- never는 타입이 두가지 일 수도 있는 상황에서도 발생할 수 있다.

### unknown

- 어떤 타입인지 모르는 변수는 타입스크립트에게 어떻게 알려줘야 할까?
- API로 부터 응답을 받는데 그 응답의 타입을 모를때 unknown을 사용할 수 있다.

```jsx
let a: unknown;

if (typeof a === "number") {
  let b = a + 1;
}
if (typeof a === "string") {
  a.toUpperCase();
}
```

- 이렇게 하면 타입스크립트로부터 일종의 보호를 해준다.
  - 어떤 작업을 하기 전에 이 타입부터 먼저 확인하는 등
  - 위처럼 if문 사용하여 확인해줘야 오류가 나지 않는다.
