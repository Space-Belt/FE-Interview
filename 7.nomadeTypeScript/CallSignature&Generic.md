# Call Signatures

```jsx
function add(a: number, b: number) {
  return a + b;
}

const sum = (a: number, b: number) => a + b;
```

- 타입을 작성하지 말아보자. 이 함수만의 타입을 만들어보자
- 우리가 할것은 call signatures다.
- call signatures라는 것은 함수 위에 마우스를 올렸을때 보게 되는 것이다.
- const sum: (a:number, b:number) => number 이런것이다.
- 이것은 내가 어떻게 함수를 호출해야하는지 알려주고 리턴타입도 알려준다.

```jsx
// 1 번
type Add = (a: number, b: number) => number;
const add2: Add = (a, b) => a + b; // => {a + b} 하면 작동하지 않는다.

// 2 번
type Add = {
  (a: number, b: number): number,
};
```

- `type Add = (a:number, b:number) => number;` 이게 signatures다.
- 타입스크립트가 유추할 수 있도록 해주는것
- 즉 타입을 만들고 함수가 어떻게 작동하는지 서술해둘 수 있다.
- 함수의 타입을 설명한 후 코드를 구현한다.
- 주석부분이 되지 않는 이유는 저 함수는 void를 반환하지 않기 때문이다.
- 2번은 call signature를 좀 더 길게 작성하는 방법이다.
- 2번과 같은 방법이 존재하는 이유는 오버로딩 때문이다. 아래에서 오버로딩을 알아보자.

# overloading(오버로딩)

- 우리는 대부분 다른 사람들이 만든 외부 라이브러리를 사용한다. 이 패키지나 라이브러리들이 오버로딩을 많이 사용한다.
- 그럼 오버로딩에 대해 알아보자!
- 오버로딩은 함수가 여러개의 call signatures를 가지고 있을 떄 발생시킨다.
  그냥 여러개가 아니라 서로 다른 여러개의 call signature를 가졌을때

### 좋지 않은 코드를 예시로 알아보자

```jsx
type Add = {
	(a:number, b:number) : number
  (a:number, b:string) : number
}

const add: Add = (a, b) => a + b;
```

- 위 코드를 보면 a + b 부분에 에러가 난다. 마우스를 올려보면 파라미터 a는 항상 number라고 나온다.
- 그리고 파라미터 b는 string이 될 수도, number가 될 수도 있다.
- 그러므로 string과 number는 더할 수 없다고 알려준다.
- 아래에서 해결해보자

```jsx
type Add = {
	(a:number, b:number) : number
  (a:number, b:string) : number
}

const add: Add = (a, b) => {
    if(typeof b === 'string') {
        return a;
    }
    return a + b;
}
```

- 이렇게 해결할 수 있다. 하지만 아주 일부의 함수만 가능하기에 좋지 않다.

### 실제로 볼 수 있는 오버로딩의 예시로 알아보자!

- 주로 우리가 사용하는 일부 라이브러리에는 몇가지 함수가 존재한다.
- 그리고 그 함수는 string을 보낼 수 있거나 configuration 객체 같은 객체를 보낼 수 있게 허용되어있다.
- NextJS를 예시로 들어보면 Nextjs는 페이지를 바꿀 수 있다.
  - Nextjs는 라우터를 가지고 있다.
  - `Router.push("/home")` 홈페이지로 이동시켜주는것이다. 또한 객체 형식으로 보내줄 수도 있다.
  ```jsx
  Router.push({
    path: "/home",
    state: 1,
  });
  ```
  - 이렇게 스트링, 객체로 보낼 수있다.
- 그렇다면 타입스크립트에선 어떻게 할까?

```jsx
type Config = {
    path: string,
    state: object
}

type Push = {
    (path: string):void // 1번
    (config: Config):void // 2번
}

const push:Push = (config) => {
    if(typeof config === "string") console.log(config);
    else {
        console.log(config.path, config.state)
    }
}
```

- 안에 내용 없을때 config를 보면 string이나 Config를 받을 수 있다고 나온다.

  <img width="240" alt="image" src="https://user-images.githubusercontent.com/82592845/166675329-03309964-56c1-405a-ac47-e3c02b98c039.png">

- 위의 예시는 흔히 일어날 수 있는 좋은 예시다.
- 패키지나 라이브러리를 디자인할 때 많은 사람들이 사용한다.
- 오버로딩은 이럴때 사용되는 것이다!
- 언제는 1번을 보내줄 수도 있고, 어쩔땐 2번을 보내줄 수도 있다.
- 즉 내가 string이나 config 타입을 가지고 있다면 타입스크립트는 내부에서 타입을 체크해준다. 위의 경우엔 config를 string인지 체크하도록 해준다.( if 문 ) else문에서는 Congif타입 객체인것을 체크한다.

- 다른 여러개의 argument를 가지고 있으면 어떤 효과가 발생할까?
- 위 코드를 예시로 들어보면, (Push) 지금은 똑같이 1개씩의 argument를 가지고 있다. 하나는 스트링 하나는 Config타입 객체다. 우린 아마 하나의 call signature는 두개의 파라미터를 가지고 다른 하나는 5개의 파라미터를 가지는 경우가 발생할 수도 있다. 아래의 코드로 알아보자!

```jsx
type Add = {
    (a: number, b:number): number
    (a: number, b:number, c:number): number,
}

const add:Add = (a,b,c?:number) => {
    return a + b
}

const add:Add = (a,b,c?:number) => {
    if(c) return a + b + c
    return a + b
}

add(1, 2)
add(1, 2, 3)
```

- 파라미터의 개수가 다르기에 c를 선택사항(Optional)으로 바꿔준것이다.
- 추가적으로 타입을 줘야한다!
- 위와같은 상황이 자주 일어나지는 않는다.

# Polymorphism ( 다형성 ) Generic이라 불림.

### Polymorphism의 뜻

- Poly는 Many,several,much,multi 의 뜻을 가졌다.
- Polygon(다각형)을 생각하면 되는데 Poly는 많은, 다수란 뜻이며 gon은 각도란 뜻이다.
- 그럼 morphos, morphic는 뭘까?
- morphos는 form(형태), structure(구조)란 뜻을 가지고 있다. 즉 형태나 구조 또는 모양이라는 뜻이다.
- 이것들을 조합해보면 many(poly) structure(morphos)가 된다.
- 즉 → 여러가지 다른 구조들이란 것이다.

기본적으로 함수는 여러가지 다른 모양을 가지고 있다.

타입스크립트에서 함수는 다른 2~3개의 parameter를 가질 수 있다. 또는 타입스크립트에서 함수는 string이나 object를 첫번째 파라미터로 가질 수 있다. 고로 위의 과정을 통해 이미 다형성을 경험해본것이다.

### 이번엔 제네릭이 어떻게 우리를 도와줄 수 있는지 알아보자.

- 배열을 받고, 그 배열의 요소를 print해주는 함수를 만들어보자.

```jsx
type SuperPrint = {
    (arr: number[]):void
    (arr: boolean[]):void
    (arr: string[]):void
}

const superPrint: SuperPrint = (arr) => {
    arr.forEach(i => console.log(i));
}

superPrint([1,2,3,4,5])
superPrint([true, false, true])
superPrint(["a","b","c"])
```

- 이렇게 할 수 있지만 더 좋은 방법으로 call signatures를 해보자.
- 위 SuperPrint의 타입들은 concrete type이 아니다.
  - concrete type은 우리가 계속 봐왔던 타입이다. (number, boolean, string, void...)
- 아래에서 타입스크립트에게 generic 타입을 받을 거라고 알려주자!
  - generic은 타입의 placeholder 같은 것이다.
  - 즉 concrete type 대신에 사용 가능하다.
  - placeholder를 작성하고 그게 뭔지 추론해서 함수를 사용하는 것이다.

```jsx

// 1번
type SuperPrint = {
    (arr: number[]):void
    (arr: boolean[]):void
    (arr: string[]):void
    (arr: (number|boolean|string)[]):void
}

superPrint([1,2,true,false]);

// 2번 generic 사용
type SuperPrint = {
    <TypePlaceholder>(arr: TypePlaceholder[]):void
}

// 3번 superPrint의 리턴타입을 바꿔보자
type SuperPrint = {
    <TypePlaceholder>(arr: TypePlaceholder[]): TypePlaceholder
}

const superPrint: SuperPrint = (arr) => arr[0]

const a = superPrint([1,2,3,4,5])
const b = superPrint([true, false, true])
const c = superPrint(["a","b","c"])
const d = superPrint([1,2,true,false,"hello"]);
```

- 어떤 배열이던 number boolean string이 아니더라도 superPrint가 잘되기 한것이다.
- 1번 방법은 모든 가능성을 다 조합해서 만들어야하기 때문에 좋은 방법은 아니다.
- 즉 generic을 사용하는 이유는 들어올 확실한 타입을 모르면 다 입력해줘야하기 때문에 사용한다. ( 확실한 타입을 모를때 사용 )
- 2번의 첫 단계는 타입스크립트에게 generic을 사용하고 싶다고 알리는 것이다.
- <> 안에는 아무것이나 들어갈 수 있다. 근데 주로 T, V 이런것들을 많이 사용한다.
  - placeholder 다
- 제내릭을 사용하면 타입스크립트가 타입을 추론해서 잘 작동시켜준다.
- 또한 제내릭은 함수에 타입을 입력하는것이 가능하다.
- 즉 ! 제내릭을 사용하면 타입을 하나하나 써줄 필요가 없다.

⇒ 여러가지 타입의 배열을 가지는 함수를 만들어봤다!

### 좀 더 쉬운 방법으로 Generic을 사용해보자.

<img width="481" alt="image" src="https://user-images.githubusercontent.com/82592845/166675349-e800e030-7cb7-47ef-9ebc-59b0c5768320.png">
- 타입스크립트가 다른 call signature를 생성해줬다.
- any와는 다르다.
- generic은 내가 요구하는 대로 signature를 생성해줄 수 있는 도구라 생각하면 좋다.

```jsx
type SuperPrint = {
  <T, M>(arr: T[], b: M): T,
};

const superPrint: SuperPrint = (arr) => arr[0];

const a = superPrint([1, 2, 3, 4, 5], "x");
```

- 두개의 argument를 받는 함수다.
- 타입스크립트는 제네릭을 처음 인식했을 때와 제네릭의 순서를 기반으로 제네릭의 타입을 알게 된다.

### 실제로 제네릭을 어떻게 다루는지 알아보자!

- 아마 우리는 제네릭을 사용하여 직접 call signature를 만드는 일은 잘 없을 것이다.
  - 우리는 주로 다른 패키지를 사용하거나, 다른 라이브러리를 사용할 것이고, 그 라이브러리들이 제네릭을 통해서 생성되기 때문이다
  - 즉 라이브러리를 만들거나, 다른 개발자가 사용할 기능을 개발하는 경우에는 제네릭이 유용하갰지만 우리가 직접 사용하는 일은 많지 않을 것이다. 그냥 사용만 할 것이다.

```jsx
function superPrint<V>(a: V[]) {
  return a[0];
}

const a = superPrint < boolean > [1, 2, 3, 4]; // 에러
```

- 위의 코드를 일반함수로 대체한 것이다.
- 타입스크립트에 덮어쓰기를 했기 때문에 오류가 발생한다. <boolean> 이 됐기때문에
- 타입스크립트가 타입을 유추하도록 하는 것이 좋다.

### 제네릭을 쓰는 다른 경우는 뭐가 있을까?

- 제네릭을 사용해 타입을 생성할수도 있고 어떤 경우는 타입을 확장할 수 있다.
- 또는 어떤 경우에 코드를 저장하기도 한다.

```jsx
type Player<A> = {
    name:string
    extraInfo:A
}

type WooPlayer = Player<{favGame:string}>

// 1번
const woo:Player<{favGame:string}> = {
    name:"woo",
    extraInfo: {
        favGame: "LOL"
    }
}

// 2번
const woo: WooPlayer = {
    name:"woo",
    extraInfo: {
        favGame: "LOL"
    }
}

// 3번
type WooExtra = {
    favGame:string
}
type WooPlayer = Player<WooExtra>

const woo:Player<{favGame:string}> = {
    name:"woo",
    extraInfo: {
        favGame: "LOL"
    }
}

// 4번
const joo: Player<null> = {
    name: "lynn",
    extraInfo:null
}
```

- 내가 원하는대로 코드를 확장 가능하다.
- 타입을 생성하고 그 타입을 또다른 타입에 넣어서 사용이 가능하다.
- 많은 것들이 있는 큰 타입이 하나 있고 그 중 하나가 달라질 수 있는 타입이면 거기게 제네릭을 넣으면 된다. 또는 커스텀한 타입을 보낼 수도 있다.

### 또 다른 사용처

- 함수에서만 쓰이는게 아니라 많은 곳에서 쓰인다.
- 대부분의 기본적인 타입스크립트의 타입은 제네릭으로 만들어져 있다.

```jsx
type C = Array<number>;

let a: C = [1, 2, 3, 4];
```

```jsx
function printAllStrings(arr: Array<number>) {} // arr: number[] 를 다르게
```

- 거의 모든곳에서 사용할 수 있다.
- 다른 프로그래밍, 프로그래머나 다른 패키지를 본다던가 할때 제네릭을 사용한 타입이 지정된 함수를 줄것이다.그럼 우린 함수에게 사용할 타입을 알려주면 된다.

- Reactjs를 타입스크립트와 같이 사용하면 useState라는 함수가 있는데 얘는 제네릭을 받는다.
- 그래서 `useState()` 이렇게 사용하면 타입스크립트는 내 state 타입을 알 수 없다.
  `useState<number>()` 이런식으로 사용해야한다.
