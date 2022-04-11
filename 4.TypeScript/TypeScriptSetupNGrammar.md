## TypeScript = JavaScript + Type 문법

---

큰 프로젝트를 사용하면 자바스크립트가 아닌 타입스크립트를 사용한다.

그 이유! 예를 들어 `7 - 3` 이 있다면 자바스크립트는 `7 - ‘3’` 도 가능하다. 자바스크립트는 Dynamic Typing을 제공하는 언어기 때문이다. (숫자 - 숫자 만 가능하지만 JS가 알아서 바꿔준다)

프로젝트 규모가 커질 수록 위와 같이 자유도가 높고 유연성이 뛰어나면 더 힘들어진다.

TypeScript는 타입을 바로바로 관리해준다. 즉 에러메세지가 정확해진다.

타입 지정 제외하면 JavaScript와 비슷하다.

---

## TypeScript 설치 & 사용방법

---

1. nodejs 설치 (최신)
2. 에디터 준비 ( ex - VScode)
3. 터미널 `npm install -g typescript`
4. 코딩할 폴더 생성, 에디터로 오픈
5. `(이름).ts` 파일 생성, 코드 작성
6. tsconfig.json 생성 후 코드 작성

   ```jsx
   {
     "compilerOptions" : {
       "target": "es5",     // 자바스크립트 몇버전일지
   		// 자바스크립트 import문법이 언제 자바스크립트 문법으로 할 것인지도 지정 가능
       "module": "commonjs",
     }
   }
   ```

   -

7. `(이름).ts` 로 코드를 짜면 브라우저에서 읽지 못한다. 그러므로 JS로 변환해야한다.

   터미널에 `tsc -w` 입력해두면 자동으로 변환된다.

   .ts 파일에 코드 작성하고 저장할때마다 자바스크립트 파일이 새로 갱신된다. 그 후 html 파일에 적용하려면 자동으로 변환된 .js파일을 사용하면 된다.

   이 변환되는 과정을 컴파일한다라고 표현한다. 그래서 컴파일할때의 옵션을 6번에서 지정해준 것이다.

---

## TypeScript 문법 알아보자!

---

### 변수 선언

- `let 이름 :string = 'woo';` 이름이라는 변수에 문자만 들어올 수 있게 지정한것
- `:string` 부분에 여러가지 변수 타입이 들어갈 수 있다. ( number, boolean, null, undefined, bigint, [], {} 등등 )
- `let 배열 :string[] = [’woo’, ‘song’];` 배열을 선언할때에는 이런식으로 어떤타입이 배열안에 들어갈 것인지 설정해야한다.
- `let 객체 :{ name : string } = { name : ‘song’ };` object 타입지정한것
  - `let 객체 :{ name? : string } = { name : ‘song’ };` 이렇게 ?표를 붙이면 “name”이라는 속성이 들어 올수도 있고 아닐수도 있다는것을 표현할 수 있다.
- `let 이름 :string | number = 'woo';` or( | )를 사용하여 다양한 타입이 들어올 수 잇도록 할 수 있다. 이것을 **Union Type** 이라 한다.
  - `let 이름 :string[] | number = 'woo';` 이런식으로 string이 담긴 Array혹은 숫자 이런식도 가능하다.
- 타입지정하는 문법이 너무 길때는 변수에 담아서도 사용 가능하다.
  - `type MyType = string | number;`
  - `let variableName :MyType = 123;` 이런식으로 가능하다.
  - 주로 대문자로 작명한다.
- 함수에서도 타입을 지정할수 있다.
  ```jsx
  function functionName(parameterName: number): number {
    return x * 4;
  }
  ```
  - 파라미터는 number 타입이어야하고 return 하는것도 number 타입이어야하는것을 지정
- Array 자료를 만들때 Array자료의 어떤 자리에는 무조건 어떤 타입이 들어와야한다. 라는 식의 지정도 가능하다
  - `type Member = [number, boolean];`
  - `let woo:Member = [];`
  - woo라는 변수에는 무조건 Member에서 선언한 타입이 들어가야한다.
  - 이것을 tuple(튜플)타입이라 한다.
  - 무조건 첫번째는 number 타입이 들어와야하고 두번째는 boolean 타입이 들어와야한다는 것을 표시한것이다.
- Object(객체)에 타입을 지정해야할 속성이 너무 많다면
  ```jsx
  type Member = {
  	 name : string,
  }
  let woo : Member = { name : 'hyuk'}  이런식이 기본인데 너무 많다면 ??

  type Member = {
  	[key :string] : string,
  }
  let woo : Member = { name : 'hyuk'}
  ```
  - `[key :string]` 모든 object의 속성이름은 string 이어야하고 들어가는것도 string 타입이어야한다고 지정한것이다.
  - 즉 글자로 된 모든 object 속성의 타입은 :stirng 타입이어야한다. 를 나타냈다.
- class 문법도 타입지정이 가능하다
  ```jsx
  class User {
  	name;
  	constructor(name) {
  		this.name = name;
  	}
  }
  - 미리 변수를 name이라는 변수를 만들어 놔야한다.
  - 그래야 constructor()에서 변수를 사용할 수 있다.

  class User {
  	name :string;
  	constructor(name :string){
  		this.name = name;
  	}
  }
  - 변수를 미리 만들어서 타입지정한것
  - constructor() 안에도 타입을 지정해주는것이 안전하다.
  ```
