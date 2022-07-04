## 문제

---

자연수 N이 주어졌을 때, 1부터 N까지 한 줄에 하나씩 출력하는 프로그램을 작성하시오.

---

## 입력

---

첫째 줄에 100,000보다 작거나 같은 자연수 N이 주어진다.

---

## 출력

---

첫째 줄부터 N번째 줄 까지 차례대로 출력한다.

---

<img width="450" alt="image" src="https://user-images.githubusercontent.com/82592845/177158333-dc6b4d9e-d798-4e1e-a4b4-3554ef396cff.png">

## 로직

---

1. 입력받아서 값을 넣는다.
2. for문으로 출력한다.

---

## 풀이

---

```jsx
const fs = require("fs");
const input = fs.readFileSync("/dev/stdin");
const a = input;
let b = [];
for (let i = 1; i <= a; i++) {
  b += i + "\n";
}
console.log(b);
```

- 쉽다..
