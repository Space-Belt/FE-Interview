# 문제 : AreThereDuplicates

<img width="435" alt="image" src="https://user-images.githubusercontent.com/82592845/169038172-e8b124e1-a100-402f-8f51-784a33b1a567.png">

### 곂치는 것이 있는지 확인하는 문제

> 풀이

```jsx
function areThereDuplicates() {
  let parameterObj = {};
  for (let val in arguments) {
    parameterObj[arguments[val]] = (parameterObj[arguments[val]] || 0) + 1;
  }
  for (let val in parameterObj) {
    if (parameterObj[val] > 1) {
      return true;
    }
  }
  return false;
}
```

- 빈도 수 세기 방법

```jsx
function areThereDuplicates(...args) {
  args.sort((a, b) => a > b);
  let start = 0;
  let next = 1;
  while (next < args.length) {
    if (args[start] === args[next]) {
      return true;
    }
    start++;
    next++;
  }
  return false;
}
```

- 다중 포인터 방법

```jsx
function areThereDuplicates() {
  return new Set(arguments).size !== arguments.length;
}
```

- Set 객체를 생성하여 알아보는 방법

  ```jsx
  function areThereDuplicates() {
    let aaa = new Set(arguments);
    console.log(arguments.length);
    console.log(aaa);
    console.log(arguments);
    return aaa !== arguments.length;
  }

  areThereDuplicates(1, 3, 3, 3, 4, 4);
  ```

  - arguments.length 는 정확히 길이가 나온다.

    <img width="537" alt="image" src="https://user-images.githubusercontent.com/82592845/169038009-0eb25cc7-80ac-4112-91a5-98ee695e6994.png">

  -
