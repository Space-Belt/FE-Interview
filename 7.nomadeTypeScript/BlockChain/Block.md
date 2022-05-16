# Blocks

- 블록코인의 몇가지 기능을 만들어 볼 것이다.
- 타입스크립트 프로젝트 할 때에 생산성을 높이는 법을 알아보자.

---

### 현재까지는 항상 `npm run build` 를 실행한 후 nodeJS로 그 코드를 실행했다.

- `npm run build` 를 하면 build라는 폴더 안에 파일을 생성해줬다.
- 이제는 `npm start` 를 실행하면 build 폴더 안의 index.js 파일을 실행하도록 해보자.

```jsx
"scripts": {
	  "build": "tsc",
	  "start": "node build/index.js"
},
```

- 여기까지 진행했다면 `npm run build && npm run start` 이런식으로 실행해야 한다.

→ 그냥 바로 실행하기 위해서 `npm i -D ts-node` 를 해준다. ( 터미널 )

→ 빌드 없이 타입스크립트를 실행할 수 있게 됐다.

→ 프로덕션에서 쓰는 패키지는 아니고 개발 환경에서만 사용하여 빌드없이 빠르게 새로고침하고 싶을떄 사용하는 것이다. ( ts-node 가 컴파일없이 타입스크립트 코드를 대신 실행해준다.

```jsx
"scripts": {
    "build": "tsc",
    "dev": "ts-node src/index",
    "start": "node build/index.js"
},
```

- dev 스크립트는 ts-node src/index.js 를 실행시킬것이다.
- 또는 `npm i nodemon` 를 설치하고 아래처럼 진행해도 된다.

```jsx
"scripts": {
    "build": "tsc",
    "dev": "nodemon --exec ts-node src/index.ts",
    "start": "node build/index.js"
},
```

- 여기까지 했다면 코드를 수정하고 저장하면 알아서 실행해준다.

---

### 이제 블록체인을 디자인해보자.

- 블록체인은 말 그대로 여러개의 블록이 사슬처럼 묶인것이다.
- 그리고 블록안에는 데이터가 존재한다. ( 블록체인으로 데이터를 보호 ) 또한 이 블록은 다른 블록에 묶여있다.
- 묶여있는 연결고리는 해쉬값이다.

### 1. 우선 Block class를 만들어주자

```jsx
interface BlockShape {
  hash: string;
  prevHash:string;
  height:number;
  data:
}

class Block implements BlockShape {
  constructor(
    public prevHash:string,
    public height:number,
    public data: string,
  ) {}
}
```

- hash ( 해쉬값 ) prevHash ( 이전 해쉬 값 ) height ( 블록의 위치를 표시해주는 숫자 ) data ( 블록이 보호할 데이터 )
- Block에 오류가 생기는데 블록에 들어갈 hash값은 constructor에 설정해주지 않았기 떄문이다.
- hash는 prevHash, height, data 값을 이용해서 계산되기 때문이다. hash는 그 블록의 고유 서명과 같은 것이다.
- 그러므로 세가지 값을 이용해 새로운 해쉬값을 생성할것이다. 이 값은 아주 멋진 결정론적인 값이다. ( 어떤 입력값의 해쉬는 언제나 같은 결과값이 나온다. )

### 2. hash 값을 생성해보자

```jsx
interface BlockShape {
  hash: string;
  prevHash:string;
  height:number;
  data:
}

class Block implements BlockShape {
    public hash: string;
  constructor(
    public prevHash:string,
    public height:number,
    public data: string,
  ) {
    this.hash =    // 여기!
  }
}
```

- (여기 !) 에서 static 함수를 사용할것인데 static 함수는 클래스 안에서 사용하는 함수다. ( 클래스 인스턴스가 없어도 부를 수 있다. )
  ```jsx
  const p = new Player();

  p.kickBall();
  ```
  - 여기 Player는 클래스고 p 는 클래스의 인스턴스다.
  - kickBall은 static 함수가 아닌 Player 클래스만 부를 수 있는 함수다.
  - 즉 p는 쉽게 말하면 살아있는 Player 같은 것이다.
  - 스태틱메소드가 아니면 살아있는 Player만 부를수 있다.
  - 하지만 static 함수는 살아있지 않은 Player도 호출할 수 있다. ( 마치 유틸리티 함수처럼 동작하는 것이다 )

```jsx
//import * as crypto from "crypto";
import crypto from "crypto";

interface BlockShape {
  hash: string;
  prevHash: string;
  height: number;
  data: string;
}

class Block implements BlockShape {
  public hash: string;
  constructor(
    public prevHash: string,
    public height: number,
    public data: string
  ) {
    this.hash = Block.calculateHash(prevHash, height, data);
  }
  static calculateHash(prevHash: string, height: number, data: string) {
    const toHash = `${prevHash}${height}${data}`;
  }
}
```

- calculateHash 함수로 hash값을 생성할 것이다.
- 그것을 위해 crypto가 필요한데 두가지 방법으로 import 해준다.
- 주석처리 안된 방식으로 사용하기 위해서는
  ```jsx
  {
    "include": ["src"],
    "compilerOptions": {
      "outDir": "build",
      "target": "ES6",
      "lib": ["ES6"],
      "strict": true,
      "allowJs": true,
      "esModuleInterop": true,
    }
  }
  ```
  - tsconfig.ts 에서 `esModuleInterop` 을 true로 설정해줘야 한다.
  - esModuleInterop
    CommonJS 모듈을 ES6 모듈 코드베이스로 가져오려고 할 때 발생하는 문제를 해결한다. ES6 모양을 준수하여 CommonJS 모듈을 정상적으로 가져올 수 있게 해준다.

---

### 설정을 조금 바꿔서 진행해보자

```jsx
{
  "include": ["src"],
  "compilerOptions": {
    "outDir": "build",
    "target": "ES6",
    "lib": ["ES6"],
    "strict": true,
    // "allowJs": true,
    "esModuleInterop": true,
    "module": "CommonJS" // umd
  }
}
```

- 강의에서는 crypto에 에러가 생기는데 전에 ts-node 패키지를 설치할 때 crypto.d.ts가 자동으로 node_modules/@types/node 경로에 생성되어서 에러가 생기지 않았다.
- 즉 ts-node 패키지를 설치하지 않았다고 가정하고 진행해보자!

<img width="527" alt="image" src="https://user-images.githubusercontent.com/82592845/168547147-323fe2f4-0334-4bf6-8291-9ed9f6c94fec.png">

- 이런 오류가 생긴다.
- 이 오류는 타입스크립트로 작성되지 않은 패키지를 import할때 타입정의가 되어있지 않을때 발생한 에러다.
- 타입스크립트에게 라이브러리의 함수를 설명해주고, 입력값 타입, 리턴 데이터 타입 설명을 `.d.ts` 파일 안에 저장했었다. → .d.ts 파일 생성 후 패키지 안에 있는 함수에 대해 타입스크립트에게 설명했다.
- 하지만 사용할때마다 .d.ts 를 만들어서 설명하기는 어렵다.
- 그래도 타입스크립트에게 패키지 안의 타입에 대해 설명해주고 타입스크립트의 보호 장치를 사용하고 싶다. 이럴때는 어떻게 해야하는지 알아보자!

### 1. DefinitelyTyped 라는 레포지토리로 가보자!

- 오직 타입정의로만 이루어져있는 레포지토리 ( [npm에 존재하는 거의 모든 패키지](https://github.com/DefinitelyTyped/DefinitelyTyped) )
- 이 레포지토리에서 원하는 .d.ts 파일을 찾아서 어떻게 사용할까?
- 콘솔을 통해 설치하면 된다! `npm i -D @types/node`
- nodejs를 위한 타입을 모두 설치해준다.
- 원하는 패키지를 다운 받은후 `npm i -D @types/(원하는패키지)`
- 최근에는 매번 설치가 필요없어졌다. 최근 패키지는 만든 사람이 자바스크립트로 작성했지만 .d.ts 파일을 함꼐 포함시킨 경우가 많기 떄문이다.
- 작업을 진행하면 crypto 도 문제없이 사용할 수 있다.

```jsx
class Block implements BlockShape {
  public hash: string;
  constructor(
    public prevHash: string,
    public height: number,
    public data: string
  ) {
    this.hash = Block.calculateHash(prevHash, height, data);
  }
  static calculateHash(prevHash: string, height: number, data: string) {
    const toHash = `${prevHash}${height}${data}`;
    return crypto.createHash("sha256").update(toHash).digest("hex");
  }
}
```

- crypto의 createHash 사용 , sha-256 방식 사용, update()를 불러 toHash의 값을 구하며, digest를 불러서 hex로 설정해준다. 이러면 선택지를 보여주는데 이것이 타입이 정의 되어있기 때문이다.
