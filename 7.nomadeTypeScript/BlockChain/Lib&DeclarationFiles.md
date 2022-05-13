# Lib Configuration

> lib 는 합쳐진 라이브러리의 정의 파일을 특정해주는 역할을 한다.
> Specify a set of bundled library declaration files that describe the target runtime environment.

- 여기서 ‘정의 파일' 이라는 말이 중요하다.
- 그러니 ‘정의 파일’의 목표로 하는 런타임 환경을 알아볼 것이다.
- “Specify a set of bundled library declaration files” 이 부분은 아직 library declaration이 뭔지 모른다.
- “that describe the target runtime environment.” 목표로 하는 실행 환경을 나타낸다고 한다.
  쉽게 말하면 나의 자바스크립트 코드가 어디에서 동작할지를 알려준다는 것이다. 즉 → 자바스크립트의 어떤 버전이 그 환경에서 사용되는지를 말해준다.
- 알아보기 위해 ES6 지원 환경에서 실행된다고 생각하자.
- `"lib": ["ES6", "DOM"]` 이 코드를 브라우저 위에서 실행한다고 해주자 → “DOM”
  1. DOM을 lib에 포함시키고, 타입스크립트 코드에서 document를 사용하면 알아서 브라우저에 맞게 여러가지 자동완성을 제공해준다.
  2. “DOM” 덕에 타입스크립트가 브라우저의 API와 타입들을 알고 있다.
  3. 타입스크립트는 Math 처럼 기본적으로 포함된 JS API에 대한 타입 정의를 가지고 있다.

### 즉 타입스크립트가 어떤 API를 사용하고 어떤 환경에서 코드를 실행할지를 지정하는 역할을 한다. ( localStorage, Math, window 등의 타입을 이해하고 인지하고 있다 )

- 그렇다면 타입스크립트는 이것들을 어떻게 알고 있는가? 에 대해 알아보자.
- declaration file 이란?
- 타입스크립트는 내장된 자바스크립트 API를 위한 기본적인 타입 정의를 가지고 있다. ⭐️
- 타입 정의가 타입스크립트가 몇몇 자바스크립트 코드와 API의 타입을 설명할 수 있도록 해준다.
- 타입 정의를 사용하는 이유는 타입스크립트를 사용하는 목적과 연관이 있다.
- 하지만 문제점은 대부분의 경우 우리는 다른 패키지, 프레임워크, 라이브러리를 사용하는데, 이 들은 자바스크립트로 만들어져 있다.
- 그래서 우리가 자바스크립트로 만들어진 라이브러리를 타입스크립트 프로젝트에 쓰려면, 타입스크립트는 그것들의 타입을 알 수 없다.
- 타입스크립트는 내가 자바스크립트 코드를 쓸 수 있도록 해준다.
- 하지만 타입스크립트에게 우리가 사용할 자바스크립트 함수의 모양을 설명하려면 타입정의가 필요한 것이다.
- 그렇다면 어떻게 타입 정의를 작성하는지 알아보자

```jsx
export function init(config) {
  return true;
}

export function exit(code) {
  return code + 1;
}
```

- myPackage.js
- 이렇게 하면 타입스크립트가 이게 뭔지 알 수 없으므로 index.ts 에서 myPackage가 node의 모듈인 것처럼 해보자 (node_module)

```jsx
{
  "include": ["src"],
  "compilerOptions": {
    "outDir" : "build",
    "target": "ES6",
    "lib": ["ES6", "DOM"],
    "strict": true
}
```

- tsconfig.json
- `strict` 타입 검사 옵션, 프로그램을 더 정확하게 보장해주고, 넓은 법위의 타입 검사를 가능하게 해준다.

```jsx
interface Config {
  url: string;
}

declare module "myPackage" {
  function init(config: Config): boolean;
  function exit(conde: number): number;
}
```

- myPackage.d.ts
- .d.ts 파일은 파일 정의가 되어 있는 곳이다. 타입스크립트가 localStorage, window 등의 모양을 아는 이유가 정의가 되어있기 때문이다.

```jsx
import { init, exit } from "myPackage";

init({
  url: "true",
});

exit(1);
```

- index.ts
- 위의 작업들을 모두 완료해야 오류없이 사용 가능하다.

### ⇒ 모듈으로 가정한 코드(자바스크립트 패키지 등 )를 사용하려면 위의 작업들을 해주어야 한다.

### ⇒ .d.ts 파일이 있기에 타입스크립트가 localStorage, window 등 여러가지를 알 수 있다.
