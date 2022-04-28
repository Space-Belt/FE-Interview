# Basic Type!!

```jsx
let age: number = 30;
let isAdult: boolean = true;
let a: number[] = [1, 2, 3];
let a2: Array<number> = [1, 2, 3];

let week1: string[] = ["mon", "tue", "wed"];
let week2: Array<string> = ["mon", "tue", "wed"];

week1.push(3); // 당연히 에러 발생 문자열 배열임
```

### 튜플(Tupple)

```jsx
let b: [string, number];

b = ["z", 1]; // 가능
// b = [1, 'z'] // 불가능

b[0].toLowerCase();
b[1].toLowerCase(); // 불가능
```

- 인덱스별로 타입이 다를때 이용 가능하다. 첫번째는 string, 두번째는 number가 들어온다.

### void, never

- void는 함수에서 아무것도 반환하지 않을때 주로 사용한다. ( no return )

```jsx
function showHello(): void {
  console.log("hello");
}
```

- never는 항상 에러를 반환하거나 영원히 끝나지 않는 함수타입으로 사용가능하다.

```jsx
function showError(): never {
  throw new Error();
}

function infLoop() {
  while (true) {
    // do something
  }
}
```

### enum 타입 (자바스크립트에는 없는 타입)

- 비슷한 값들끼리 묶여있다고 생각하면 된다.

```jsx
enum Os {
	Window,
	Ios,
	Android
}

// 컴파일 된 결과  (양방향 래핑)
var Os;
(function (Os) {
	Os[Os["Window"] = 3] = "Window";
	Os[Os["Ios"] = 10] = "Ios";
	Os[Os["Android"] = 11] = "Android";
})(Os || (Os = {}));

enum Os {
	Window = 'win',
	Ios = 'ios',
	Android = 'and'
}

var Os;
(function (Os) {
	Os["Window"] = "Window";
	Os["Ios"] = "Ios";
	Os["Android"] = "Android";
})(Os || (Os = {}));

// 즉 이런모습으로 컴파일 된다.
const Os = {
	Window : 'win',
	Ios : 'ios',
	Android : 'and'
}
```

- Window, Ios, Android 에 마우스를 올려보면 window는 0 , Ios는 1, Android는 2라고 값이 할당된다. 이렇게 enum에 수동으로 값을 할당해주지 않으면 자동으로 0부터 1씩 증가하면서 할당 된다.
- Window에 Window = 3 이렇게 값을 할당해주면 Ios, Android에는 4, 5 가 할당 된다.
- Ios에 10을 할당해주면 Android에는 11이 할당 된다.
- 컴파일된 결과를 보면 양방향래핑이 되어있다.
  - 이것이 무슨 의미?
  - console.log(Os[10])을 해보면 “Ios”가 나온다.
  - console.log(Os[’Ios’])를 해보면 10이 반환된다.
- enum에는 숫자가 아닌 문자열도 입력이 가능하다.
  - 이럴때는 양방향 래핑이 아닌 단방향 래핑이 된다.
- 위를 이용하여 다른것을 알아보자

```jsx
enum Os {
	Window = 'win',
	Ios = 'ios',
	Android = 'and'
}

let myOs:Os;

myOs = Os.window   // 이렇게 사용한다.
```

- 이렇게 선언하면 Window, Ios, Android만 입력할 수 있게 된다.
- enum은 특정값만 입력하도록 감지하고 싶거나, 그리고 그 값들이 뭔가 공통점이 있을때 enum을 사용하면 된다.

### null, undefined

```jsx
let a: null = null;
let b: undefined = undefined;
```

- 이렇게 선언한다.
