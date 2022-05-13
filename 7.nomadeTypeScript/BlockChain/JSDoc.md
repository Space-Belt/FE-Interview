# JSDoc

- 우리는 프로젝트에 자바스크립트 파일과 타입스크립트 파일이 같이 들어있는 경우를 자주 마주하게 된다.
- `import { init, exit } from "./myPackage";` 이번엔 타입스크립트 파일에 myPackage 파일을 불러와보자. ( ./ ) 추가
- tsconfig.json 파일이 `"allowJS":true` 를 더해준다.
  - 타입스크립트가 js 파일안에 들어가 함수를 다 불러올 수 있도록 해주는것
- 위의 과정을 거치고 나면 타입스크립트와 자바스크립트가 공존한다.

---

- 타입스크립트 파일이 자바스크립트 파일을 확인하게도 하고 싶고 완전히 타입스크립트로 바꾸고 싶지 않다면 아래의 과정을 따라가자.
- 먼저 왜 이런 상황이 되는가?
  - 코드가 엄청 많은 프로젝트가 있다고 가정하면 당장 코드를 변경하거나, 삭제하고 싶지 않을 것이다. 코드가 엄청 많을때엔 그냥 자바스크립트 파일인 채로 두는것이 좋다. 그냥 타입스크립트의 보호를 받고 싶을뿐이면 타입스크립트가 자바스크립트 파일도 보호해준다!

1. 자바스크립트에 보호 장치를 더하려면 코멘트를 더해야한다. ( myPackage.js )

   ```jsx
   // @ts-check
   /**
    * Initializes the project
    * @param {object} config
    * @param {boolean} config.debug
    * @param {string} config.url
    * @returns boolean
    */
   export function init(config) {
     return true;
   }

   /**
    * Exits the program
    * @param {number} code
    * @returns number
    */
   export function exit(code) {
     return code + 1;
   }
   ```

   - `@ts-check`타입스크립트 파일에게 자바스크립트 파일을 확인하라고 알리는것
   - 타입스크립트가 제공하는 보호장치를 사용하기 위해선 `/** */` 안에 코멘트를 남겨줘야한다. → 이것을 JSDoc 이라한다.
   - 코멘트로 이루어진 문법이다.
   - 제대로 작성하면 타입스크립트가 이 코멘트를 읽을 수 있다.
   - 타입스크립트가 코멘트를 읽고 타입을 확인해줄것이다.
   - 자바스크립트 파일안에 JSDoc 코멘트를 더해주면 알아서 보호기능을 사용할 수 있다.

   ```jsx
   import { init, exit } from "./myPackage";
   ```

   <img width="424" alt="image" src="https://user-images.githubusercontent.com/82592845/168304685-6853d34b-b265-45d9-86b0-9ac1d7117494.png">

   <img width="419" alt="image" src="https://user-images.githubusercontent.com/82592845/168304721-8861487e-4b8f-4472-940e-1291e6f57a57.png">
