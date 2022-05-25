### SSR ( SERVER SIDE RENDERING ) CRA - REACT

- 브라우저가 유저가 보는 UI를 만드는 모든 것을 함
- 소스코드를 확인해보면 `<div id="root"></div>` 하나만 있다. 나머지는 자바스크립트다.
- 브라우저가 react.js를 다운받고 코드를 다운받았을때 React.js 는 다른 모든것들을 렌더링하고 유저는 네비게이션 바등을 비롯한 나머지를 보게 된다.
- 이것이 client-side-render가 의미하는 것이다. ( 브라우저가 자바스크립트 가져와 client-side의 자바스크립트가 모든 UI를 만드는 것이다. )
- 브라우저가 HTML을 가져올 때 비어있는 div로 가져온다는것을 의미
- 브라우저가 `<div id="root"></div>` 를 받아오고 `react.js` 받아와서 자바스크립트, react 등 모든것을 fetch 한 후 에 UI가 보인다.
- 우리가 hello 가 나오게 한 페이지를 자바스크립트를 허용하지 않는 설정 후 새로고침 하여 소스코드를 보면 실제 HTML 코드가 보인다. 그리하여 유저가 느린 연결을 하고 있거나, 자바스크립트가 완전히 비활성화 되어있어도 유저는 적어도 HTML을 볼 수 있다.
- API로 부터 데이터를 로딩하거나 하는데 오랜시간이 걸리기에 사용하면 좋을것 같다.
- next.js 에 의해 미리 생성되는것 , 유저가 페이지 요청하면 진짜 HTML을 얻고, 로딩전에 보이는것이 있다.

---

### 기능 hydration 을 살펴보자

- counter 생성
  ```jsx
  import { useState } from "react";

  export default function Home() {
    const [counter, setCounter] = useState(0);
    return (
      <div>
        <h1>Hello {counter}</h1>
        <button onClick={() => setCounter((prev) => prev + 1)}>+</button>
      </div>
    );
  }
  ```
  - 소스코드를 보면 내가 만든 초기 상태를 활용해 미리 렌더링 되어 있다. (HTML)
  - 이것을 pre-rendering이라 한다.
  - next.js는 초기 상태로 pre-rendering 을 하는 것이다.
  - 소스코드를 보면 next.js의 아주 좋은점을 알 수 있다. 페이지가 로딩될때 정말 많은 스크립트를 같이 요청한다. 좋은 점은, 페이지가 로딩됐을때, react.js가 넘겨받아서 잘 작동한다.
  - 페이지를 열면 , 보게되는것은 그냥 HTML 파일이고 그다음 react.js가 클라이언트로 전송됐을 때, react.js 앱이 되는것이다.
  - 여기서 react.js를 프론트엔드 안에서 실행하는 것을 hydration이라 한다.
  - 이유는 next.js는 react.js를 백엔드에서 동작시켜 이 페이지를 미리 만드는데, 이게 component 들을 렌더링 하고 렌더링이 끝났을때 그것들은 HTML이 되며, next.js는 그 HTML을 페이지의 소스코드에 넣어준다. ⇒ 그럼 유저는 자바스크립트와 react.js가 로딩되지 않았어도 콘텐츠를 볼 수 있다.
  - 그리고 react.js가 로딩됐을때, 기본적으로 이미 존재하는 것들과 연결이 되어서, 일반적인 react.js 앱이 된다.
  - 먼저 렌더링 되기 때문에 SEO에 좋다. (검색엔진에게도 좋고 유저에게도 좋다)
