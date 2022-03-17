> Document Object Model (DOM)

### 😄 먼저 DOM을 이해 하기 위해서 웹 페이지가 렌더링되는 과정을 간단히 알아보자

---

1. 브라우저가 HTML을 만나면 파싱하여 DOM 트리를 생성한다.
2. 브라우저가 CSS를 만나면 파싱하여 CSS Object Model(CSSOM) 트리를 생성한다.
3. 브라우저가 JavaScript를 다운로드, 실행한다.
4. DOM과 CSSOM을 결합하여 렌더 트리를 생성한다.
5. 렌더트리를 이용하여 HTML요소들의 레이아웃을 계산하여 화면에 그려주는 페인팅 작업을 한다.

---

- 렌더링 되는 과정속에서 렌더트리를 구축하는 재료가 되어주는 것이 DOM인 것을 확인 할 수있다.

---

### 🎋그럼 DOM이 어떤것인지 알아보자

> DOM은 왜 만들어 졌을까?

- 옛날에는 웹페이지를 정적으로 만들수 밖에 없어서 제한이 많았다. 그래서 여러 뛰어난 개발자들이 모여 HTML을 철저히 연구하여 HTML의 거의 모든부분까지 접근할 수 있는 DOM을 만들어 낸것이다.
- 그리하여 HTML로 구성된 웹 페이지를 동적으로 움직이게 만들 수 있게 되었다.

---

> DOM은 HTML로 부터 생성되지만 차이점이 있다.

- HTML → 화면에 보이기위해 규칙에 따라 정해진 태그, 속성값으로 이루어진 문서 (단순 텍스트)
- DOM → 브라우저가 HTML 파싱 후 node, property, method를 가지는 객체로 생성된 것이며, 스크립트, CSS등의 언어들로 DOM 구조에 접근, 제어, 커스텀 등 사용 방법이 많다.

---

> DOM(문서객체모델)이 무엇인가?

- 웹 페이지에 대한 프로그래밍 인터페이스라고 할 수 있다. 즉 HTML요소를 Object처럼 조작할 수 있는 Model이다. 그러니 자바스크립트를 사용할줄 안다면 DOM을 이용해 HTML을 조작할 수있다.
- 다양한 환경과 어플에서 사용 가능한 API라고 설명하기도 한다.

참고 : [https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)
