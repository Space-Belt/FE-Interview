# useInput

> useInput은 기본적으로 input을 업데이트 한다.

### 기본 형

```jsx
const useInput = (initialValue) => {
  const [value, setValue] = useState(initialValue);
  return { value };
};
```

- useInput은 initialValue를 받는다.
- 그리고 useState를 사용하여 initialValue를 받는 value를 만든다.

### 1. 사용 해보자

1-1.

```jsx
export default function App() {
  const name = useInput("Mr.");
  return (
    <div className="App">
      <h1>HI!</h1>
      <input placeholder="Name" value={name.value} />
    </div>
  );
}
```

- useInput을 사용하여 Mr. 를 이름의 기본값으로 설정했다. ( Mr. [입력한이름] )이 되도록
- 그럼 이제 여기에 hooks을 사용해볼 것이다.
- useInput은 value를 return 하고, name은 value와 같아지고, 그 다음 input의 value는 name.value를 가지게 된다.
- `<input placeholder=”Name” value={name.value} />` 는 아래와 같이 shortcut으로 바꿀수 있다. `<input placeholde=”Name” {...name} />`

<img width="371" alt="image" src="https://user-images.githubusercontent.com/82592845/164430546-171ddae4-3e3d-4404-b124-7157b428a20f.png">

1. 값을 변경해 줄 수 있도록 바꿔보자!

```jsx
const useInput = (initialValue) => {
const [value, setValue] = useState(initialValue);
const onChange = (event) => {
console.log(event.target);
};
return { value, onChange };
};

아래의 코드로 변경

const useInput = (initialValue) => {
const [value, setValue] = useState(initialValue);
const onChange = (event) => {
const {
target: { value }
} = event;
setValue(value);
};
return { value, onChange };
};

export default function App() {
const name = useInput("Mr.");
return (
<div className="App">
<h1>HI!</h1>
<input placeholder="Name" {...name} />
******<input placeholde="Name" value={name.value} onChange={name.onChange} />***
</div>
);
}
```

- 이렇게하면 콘솔에 입력하는 값들이 보여진다.
- 이것은 다른 function에서 이벤트를 처리 할수 있다는 것을 보여준다. (react component가 아니라 완전히 다른 function 이다)
- 이벤트를 분리된 파일, 다른 entity에 연결해서 처리 할 수 있기도 하다.

---

## 🙌🏻useInput 파고들기

```jsx
const useInput = (initialValue) => {
  const [value, setValue] = useState(initialValue);
  const onChange = (event) => {
    const {
      target: { value },
    } = event;
    setValue(value);
  };
  return { value, onChange };
};

export default function App() {
  const name = useInput("Mr.");
  return (
    <div className="App">
      <h1>HI!</h1>
      <input placeholder="Name" {...name} />
    </div>
  );
}
```

## useInput을 좀 더 확장시켜보자 !

```jsx
const useInput = (initialValue, validator) => {
  const [value, setValue] = useState(initialValue);
  const onChange = (event) => {
    const {
      target: { value },
    } = event;
    let willUpdate = true;
    if (typeof validator === "function") {
      willUpdate = validator(value);
    }
    if (willUpdate) {
      setValue(value);
    }
  };
  return { value, onChange };
};

export default function App() {
  const maxLen = (value) => value.length <= 10;
  const name = useInput("Mr.", maxLen);
  return (
    <div className="App">
      <h1>HI!</h1>
      <input placeholder="Name" {...name} />
    </div>
  );
}
```

1. initialValue뿐만 이 아니라 유효성을 검사하는 validator를 추가하자

- validator에는 maxLen이 들어간다. value.length ≤ 10가 되어 value가 false가 되면 더이상 입력 할 수 없게 된다.
- maxLen이 유효성을 검사하는 기준을 제시한다.
- !value.includes(”@”); 로 바꾼다면 @를 입력할 수 없게 된다.
