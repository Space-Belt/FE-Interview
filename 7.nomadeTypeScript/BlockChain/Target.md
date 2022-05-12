# Targets

---

### 타입스크립트 설치 방법

`npm i -D typescript`

### Package.json 초기화

`npm init -y`

### tsconfig.json 설정 [참고](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#handbook-content)

directory에 tsconfig.json 파일이 있으면 해당 directory가 TypeScript 프로젝트의 루트임을 나타낸다. tsconfig.json 파일은 프로젝트를 컴파일하는 데 필요한 루트파일과 컴파일러 옵션을 지정한다.

### Target( 기본값: ES3 )

최신 브라우저는 모든 ES6 기능을 지원한다. 코드가 이전버전에 배포된 경우 낮은 target을 설정하거나 최신환경에서 코드 실행이 가능하다면 더 높은 target을 설정하도록 선택가능하다.

ESNext 값을 Type

### 1. typechain 폴더 생성

### 2. `npm init -y` → package.json 생성

### 3. package.json main 삭제, script 내 test 삭제

### 4. `npm i -D typescript` D로서 devDependencies에 설치 된다.

### 5. src 폴더 생성, index.ts 파일 생성

### 6. `const hello = () ⇒ “hi”;` 코드 작성 → 이 파일 컴파일 해보자!

- 이 파일에 타입스크립트를 실행하여, 자바스크립트 파일을 받아보자.

### 7. `touch tsconfig.json` 터미널에 입력하여 파일 생성

- touch 가 생성 명령
- tsconfig.json이 있으면 vs code는 우리가 타입스크립트로 작업한다는것을 알게되며, 자동완성기능을 제공한다.

### 8. tsconfig.json 파일에 코드 입력

1. 어디에 타입스크립트 파일이 위치하는지 알려준다.(include)
   1. 우리가 자바스크립트로 컴파일하고 싶은 모든 디렉터리를 배열안에 넣어준다.
   2. src 는 타입스크립트가 src의 모든 파일을 확인한다는 것을 의미한다.
2. compilerOptions 지정
   1. {} 안에 outDir 라는 또 다른 키 생성
   2. outDir는 보통 자바스크립트 파일이 생성될 디렉터리를 지정한다. ( 타입스크립트는 컴파일러라, 이 파일들을 일반적인 자바스크립트로 컴파일 시켜준다. )
   3. outDir 를 build로 설정
3. package.json의 script 에 `"build": "tsc"` 추가

### 9. terminal에 `npm run build` 하면 tsc 작동 → build 폴더 생성 됨

### 10. 6번에서 작성했던 화살표 함수가 자바스크립트로 바뀌면서 일반함수로 바뀌어있다.

- 또한 자바스크립트는 const 대신 var를 사용한다.
- 즉 타입스크립트가 이 코드를 컴파일해서 낮은 버젼의 자바스크립트 코드로 바꿔준다.( 어떤 환경에서도 이해할 수 있는 호환성 좋은 자바스크립트 코드로 )

### 11. 코드가 어떤 버전의 자바스크립트로 바뀔지 정하는 법

- tsconfig.json compilerOptions에 `"target": "ES3"` 설정으로 버젼을 지정한다.
- ES6 에서 새로 출시된 const let 화살표 함수 ( 이상적이다.)
- 브라우저를 만드려면 ES6 를 추천
- NODEJS로 백엔드를 위한 자바스크립트 코드를 만들땐, 배포하고 싶은 환경에 맞춰 더 낮은 버젼을 target으로 설정
- ex) 서버를 운영하는데 서버를 타입스크립트로 만들고 싶고, 내가 엄청 오래된 서버, 즉 구버전의 node.js를 사용하는 경우에는 나의 node.js 버전과 호환되는 버전으로 직접 바꿔줘야 한다. 하지만 우리가 사용하는 대부분의 server provider에서는 문제가 없을 것이다.

### 만약 CRA나 Next.js, NestJS같은 프레임워크를 사용하면 알아서 해주지만 target 이 뭔지 알고 있자
