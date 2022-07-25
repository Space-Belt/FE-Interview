## 문제

---

두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오.

---

## 입력

---

입력은 여러 개의 테스트 케이스로 이루어져 있다.

각 테스트 케이스는 한 줄로 이루어져 있으며, 각 줄에 A와 B가 주어진다. (0 < A, B < 10)

---

## 출력

---

각 테스트 케이스마다 A+B를 출력한다.

---

<img width="330" alt="image" src="https://user-images.githubusercontent.com/82592845/180777790-702806bc-b114-4fb7-bb8a-4e53d231fbaf.png">

## 로직

---

1. 입력받기
2. 출력하기

- 입력값에 개행값이 있기때문에 원래길이보다 -1 하거나 처음에 trim()을 사용해서 공백을 없애준다.

---

## 풀이

---

1. 길이에서 -1 뺸거

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const last = input.length;
let j = 0;
let result = [];
while (j < last - 1) {
  let a = Number(input[j].split(" ")[0]);
  let b = Number(input[j].split(" ")[1]);
  let c = a + b;
  console.log(c);
  j++;
}
```

2. 함수만들어서 풀기

```jsx
const answer = (arr) => {
  return arr
    .map((num) => {
      return +num.split(" ")[0] + +num.split(" ")[1];
    })
    .join("\n");
};

let fs = require("fs");
let input = (require("fs").readFileSync("/dev/stdin") + "").trim().split(`\n`);
console.log(answer(input));
```
