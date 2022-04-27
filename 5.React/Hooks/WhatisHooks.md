# Hooks ???

### Hook의 역사는 Recompose에서 시작됐다.

1. 결론적으로 functional component에서 state를 가질 수 있게 해주는 것이다.
2. 앱을 리액트 훅으로 만든다면, class component, did mount, render 이런것들을 하지 않아도 된다. ( 즉 모든것은 하나의 function이 되는 것이다. ⇒ 이제 함수형 프로그래밍 스타일이 된다는 뜻 )
3. Hooks는 함수형 프로그래밍이고, state를 준다.
4. recompose + 리액트팀의 결합이 바로 리액트 훅 이라고할 수 있다.

### 간단한 예시를 보자!

- 버튼을 누르면 count가 증가하는 예시

```jsx
import React, { Component } from "react";

class App extends Component {
  state = {
    count: 0,
  };
  modify = (n) => {
    this.setState({
      count: n,
    });
  };
  render() {
    const { count } = this.state;
    return (
      <>
        <div>{count}</div>
        <button onClick={() => this.modify(count + 1)}>Increment</button>
      </>
    );
  }
}

export default App;
```

- Class component가 필요하고 State도 필요하고, 그것을 정리하고, 패스하는 등등의 과정이 있다.

### 위 예시를 react hook을 사용하면 어떻게 변할까? 어떻게 이것을 함수형 프로그래밍으로 바꿀까? 클래스를 벗어나서 함수에 머물 수 있는 방법은? this를 안하는 방법은??

```jsx
import React, { Component, useState } from "react";

const App = () => {
  const [count, setCount] = useState(0);
  const [email, setEmail] = useState("");
  const updateEmail = (e) => {
    const {
      target: { value },
    } = e;
    setEmail(value);
  };
  return (
    <>
      {count}
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <input placeholder="Email" value={email} onChange={updateEmail} />
    </>
  );
};

export default App;
```

- useState는 value와 value를 변경하는 방법을 하나 준다.
- email도 입력할 수 있도록 해주었다.
- 이렇게 간단하게 표현할 수 있다.

## 위에 처럼 Hook을 사용하여 간단하게 나타낼 수 있다.

### 훅에는 Effect Hook (useEffect )라는 것도 있는데, 한번 알아보자!

- 이것은 주로 API에서 데이터를 요청할때 사용한다.
