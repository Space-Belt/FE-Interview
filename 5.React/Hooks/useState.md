# useState

---

> Hook은 react의 state machine에 연결하는 기본적인 방법이다.

훅을 사용하면 더 이상 class를 사용하지 않고 모든것이 함수형 프로그래밍이 되는것이다

>

### useState ( 가장 기본적인 hook )

`const [item, setItem] = useState(1)`

- 이런식으로 value 와 value를 바꿀수 있는 함수를 배열에 생성해준다.
- useState() 괄호안에 들어가는 것은 기본값이다. 필요시에 기본값을 준다.

### 간단한 예시를 보자! ( 옛 방법 → 새로운 방법 )

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
