# 빠른 A + B

## 문제

---

본격적으로 for문 문제를 풀기 전에 주의해야 할 점이 있다. 입출력 방식이 느리면 여러 줄을 입력받거나 출력할 때 시간초과가 날 수 있다는 점이다.

또한 입력과 출력 스트림은 별개이므로, 테스트케이스를 전부 입력받아서 저장한 뒤 전부 출력할 필요는 없다. 테스트케이스를 하나 받은 뒤 하나 출력해도 된다.

자세한 설명 및 다른 언어의 경우는 [이 글](http://www.acmicpc.net/board/view/22716)에 설명되어 있다.

---

## 입력

---

첫 줄에 테스트케이스의 개수 T가 주어진다. T는 최대 1,000,000이다. 다음 T줄에는 각각 두 정수 A와 B가 주어진다. A와 B는 1 이상, 1,000 이하이다.

---

## 출력

---

각 테스트케이스마다 A+B를 한 줄에 하나씩 순서대로 출력한다.

---

<img width="710" alt="image" src="https://user-images.githubusercontent.com/82592845/176177130-ed31ebbf-82dd-4b40-b587-7a5d656a31c3.png">

## 로직

---

1. fs를 이용해 입력을 받는다
2. for문 이용해서 배열에 넣는다
3. 출력이 오래 걸리지 않게 값을 받는 배열에 다시 넣는다.
4. 출력

---

## 풀이

---

1. 첫번째 풀이

```jsx
const fs = require("fs");
const input = fs.readFileSync("/dev/stdin").toString().split("\n");

let count = Number(input[0]);
let sum = [];
for (let i = 1; i <= count; i++) {
  let a = Number(input[i].split(" ")[0]);
  let b = Number(input[i].split(" ")[1]);
  sum += a + b + "\n";
}
console.log(sum);
```

2. 두번째 풀이

```jsx
const readline = require("readline");
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

let input = [];

let sum = [];
rl.on("line", function (line) {
  input.push(line);
}).on("close", function () {
  let count = Number(input[0]);
  for (let i = 1; i <= count; i++) {
    let a = Number(input[i].split(" ")[0]);
    let b = Number(input[i].split(" ")[1]);
    sum += a + b + "\n";
  }
  console.log(sum);
});
```
