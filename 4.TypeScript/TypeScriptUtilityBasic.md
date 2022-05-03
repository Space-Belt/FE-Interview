# TypeScript Utility Types

1. keyof

```jsx
interface User {
	id: number;
	name: string;
	age: number;
	gender: "m" | "f";
}

type UserKey = keyof User; // id, name, age, gender

cosnt  uk:UserKey = "age"; // User 인터페이스의 키값이 들어가야한다 " " 안에
```

- keyof를 사용하면 User 인터페이스의 키값들을 union 형태로 받을 수 있다.

1. Partial<T>

```jsx
interface User {
  id: number;
  name: string;
  age: number;
  gender: "m" | "f";
}

//interface User {
//	id?: number;
//	name?: string;
//	age?: number;
//	gender?: "m" | "f";
//} 와 같다.

let admin: Partial<User> = {
  id: 1,
  name: "Bob",
};
```

- Partial은 프로퍼티를 모두 옵션으로 바꿔준다. 그래서 일부만 사용하는게 가능하다.
- 주석처리한것과 같은 효과이다. 그러므로 없는 프로퍼티를 사용하면 오류가 발생한다.

1. Required<T>

```jsx
interface User {
  id: number;
  name: string;
  age?: number;
}

let admin: User = {
  id: 1,
  name: "Bob",
};
```

- Partial과 반대로 모든 프로퍼티를 필수로 바꿔준다.
- let admin: Required<User> = {} 하면 모두 필수요소로 만들어 모두 사용해야한다.

1. Readonly<T>

```jsx
interface User {
  id: number;
  name: string;
  age?: number;
}

let admin: Readonly<User> = {
  id: 1,
  name: "Bob",
};

admin.id = 4; //  원래는 이렇게 변경이 가능하지만 Readonly<T>를 사용하면 에러가 발생한다.
```

- 말그대로 읽기전용속성으로 바꾸는것.

1. Record<K,T> ( K = Key , T = Type )

```jsx
//interface Score {
//	"1": "A" | "B" | "C" | "D";
//	"2": "A" | "B" | "C" | "D";
//	"3": "A" | "B" | "C" | "D";
//	"4": "A" | "B" | "C" | "D";
//}

type Grade = '1' | '2' | '3' | '4';
type Score = 'A' | 'B' | 'C' | 'D';

//const score: Record<'1'|'2'|'3'|'4', 'A' | 'B' | 'C' | 'D'> = {
const score: Record<Grade, Score> = {
	1: "A",
	2: "C",
	3: "B",
	4: "D",
}

// 2번 예제

interface User {
	id: number;
	name: string;
	age: number;
}

function isValid(user:User) {
	const result: Record<keyof User, boolean> = {
		id : user.id > 0,
		name : user.name !== ''.
		age : user.age > 0
	}
	return result;
}
```

1. Pick<T,K>

```jsx
interface User {
  id: number;
  name: string;
  age: number;
  gender: "M" | "W";
}

const admin: Pick<User, "id" | "name"> = {
  id: 0,
  name: "Bob",
};
```

- T/Type 에서 K 프로퍼티만 골라서 사용한다.

1. Omit<T,K>

```jsx
interface User {
  id: number;
  name: string;
  age: number;
  gender: "M" | "W";
}

const admin: Omit<User, "age" | "gender"> = {
  id: 0,
  name: "Bob",
};
```

- T타입의 K프로퍼티를 생략하고 사용하는것

1. Exclude<T1,T2>

```jsx
type T1 = string | number | boolean;
type T2 = Exclude<T1, number | string>; // boolean 만 남음
```

- T1 / Type1 에서 T2/ Type2 를 제외하고 사용하는것
- Omit은 프로퍼티들을 제외하는것이고 Exclude는 타입으로 제거한다.
- T1의 타입들중 T2타입과 겹치는 타입을 제외 시킨다.

1. NonNullable<Type>

```jsx
type T1 = string | null | undefined | void;
type T2 = NonNullable<T1>;
```

- Null 과 undefined를 제외
- string과 void만 남아있게 된다.
