# TypeScript Interface

---

> 코드로 알아보자

```jsx
let user: object;

user = {
  name: "xx",
  age: 30,
};

console.log(user.name); // err
```

- 유저라는 객체를 선언,
- 값을 할당 했는데 콘솔을 찍어보면 에러가 발생한다.
  - 오류를 확인해보면 객체에는 특정 속성값에 대한 정보가 없기 때문이다.
  - name에는 object에는 name이 없다고 뜬다.
  - 이렇게 프로퍼티를 정해서 객체로 표현하자고 할때에는 인터페이스를 사용한다.

```jsx
interface User {
  name: string;
  age: number;
}

let user: User = {
  name: "xx",
  age: 30,
};

user.age = 10;

console.log(user.name); // xx
```

- let user : User = {} 이렇게 하면 오류가 발생한다. (name과 age 프로퍼티가 있어야 한다는 오류)
  - 값을 할당해주고 콘솔을 찍어보면 잘 출력된다.
- user.age에 10 을 재할당 해주는것은 가능하다.

### Optional Property

```jsx
interface User {
  name: string;
  age: number;
  gender?: string;
}

let user: User = {
  name: "xx",
  age: 30,
};

user.gender = "male"; // 오류
```

- interface에 gender? ← 이 물음표를 붙이면 Optional 해지기 때문에 있어도 되고 없어도 되는프로퍼티가 된다.
- 만약 gender라는 프로퍼티가 있을 때는 반드시 String이어야 한다.

### readOnly를 알아보자

```jsx
interface User {
	name: string;
	age: number;
	gender? : string;
//	birthYear : number; 이렇게하면 수정이 가능하다.
	readonly	birthYear : number;
}

let user : User = {
	name : 'xx',
	age : 30,
//	birthYear : 2000, 이렇게하면 수정이 가능하다. 밖에서 user.birthYear= 100;
	birthYear : 2000,
}

```

- readonly를 부여하면 선언할때만 할당이 가능하고 그 이후에는 수정이 불가능해진다.
- 유저 인터페이스를 사용하다보면 추가해야할 것들이 생길 수 있다. 아래에서 알아보자!

```jsx
interface User {
	name: string;
	age: number;
	gender? : string;
	readonly	birthYear : number;
	1: string;
	2: string;
	3: string;
	4: string;
}

let user : User = {
	name : 'xx',
	age : 30,
	birthYear : 2000,
	1 : 'A'
}
```

- 학년별로 점수를 기입하고 싶다 할때에 이렇게 인터페이스를 정해둘 수 있다.
- 이렇게하면 user에서 선언해주지 않으면 오류가 발생하므로 ? 를 붙여주는 방법을 사용한다.
- 그 외의 방법을 아래에서 알아보자!

```jsx
interface User {
	name: string;
	age: number;
	gender? : string;
	readonly	birthYear : number;
	[grade:number] : string;  // grade라는 것에 의미는 없으니 다른문자로 바꿔도 된다.
}

let user : User = {
	name : 'xx',
	age : 30,
	birthYear : 2000,
	1 : 'A',
	2 : 'B'
}
```

- 문자열 인덱스 설명을 추가하는 방법이다.
- [grade:number] : string; → 그냥 넘버를 키로 하고 스트링을 value로 받는 프로퍼티를 여러개 가져올 수 있다는 뜻이다.
- 이렇게 하면 문제가 발생하지 않는다.
- 하지만 이렇게 string으로 grade를 받기에는 범위가 너무 넓다. 이럴때 사용하는게 문자열 리터럴 타입이다. 아래에서 알아보자

```jsx
type Score = 'A' | 'B' | 'C' | 'F';

interface User {
	name: string;
	age: number;
	gender? : string;
	readonly	birthYear : number;
	[grade:number] : Score;
}

let user : User = {
	name : 'xx',
	age : 30,
	birthYear : 2000,
	1 : 'A',
	2 : 'D'   -> 에러 발생
}
```

- 이렇게하면 Score에 있는 값들만 사용이 가능하다.

### Interface로 함수 정의하기

```jsx
interface Add() {
	(num1:number, num2: number): number;
}
// () : number; 기본형

const add : Add = function (x, y) {
	return x + y;
}

add(10, 20);
```

- const add : Add = function () {} 이렇게만 하면 인터페이스에서 정의한 함수와 모습이 달라서 오류가 발생한다!
- 이렇게 잘 사용이 가능하다. 타입 지키기 !

### 위와 비슷하게 나이를 받아서 boolean 값을 리턴하는 함수를 만들어 보자

```jsx
interface IsAdult() {
	(age:number):boolean;
}

const a:IsAdult = (age) => {
	return age > 19;
}

a(22); // true
```

- 19보다 크면 true, 작으면 false 반환

### Interface로 Class 정의 해보기

- 이때는 implements라는 키워드를 사용한다. (자바와 씨언어는 익숙)

```jsx
interface Car {
  color: string;
  wheels: number;
  start(): void;
}

class Bmw implements Car {
  color;
  wheels = 4;
  // color = "red";  이것을 constructor를 이용해서 한것 ( 생성될때 색상 입력 )
  constructor(c: string) {
    this.color = c;
  }

  start() {
    console.log("go...");
  }
}

const b = new Bmw("green");
console.log(b);
b.start();
```

- car라는 인터페이스를 정하고 이것을 이용하여 Bmw라는 클래스를 만든것이다.
- 위에 있는 속성값들을 전부 입력해야한다.

### Interface의 확장 ( extends)

```jsx
interface Car {
	color: string;
	wheels: number;
	start(): void;
}

interface Benz extends Car {
	door: number;
	stop(): void;
}

const benz : Benz = {
	door : 5,
	stop() {
		console.log('stop');
	}
	color: 'black',
	wheels: 5,
	start(){},
```

- 상속받는것이다. Car의 프로퍼티를 그대로 다 받는다.

### 참고로 확장은 여러개를 할 수 있다.

```jsx
interface Car {
  color: string;
  wheels: number;
  start(): void;
}

interface Toy {
  name: string;
}

interface ToyCar extends Car, Toy {
  price: number;
}
```

- 이렇게 동시에 확장하여 두개를 다 사용할 수 있다.
