# 벌집

## 문제

<img width="339" alt="image" src="https://user-images.githubusercontent.com/82592845/166928207-bf3a24e7-74bd-4b86-9688-75db697c87bf.png">

위의 그림과 같이 육각형으로 이루어진 벌집이 있다. 그림에서 보는 바와 같이 중앙의 방 1부터 시작해서 이웃하는 방에 돌아가면서 1씩 증가하는 번호를 주소로 매길 수 있다. 숫자 N이 주어졌을 때, 벌집의 중앙 1에서 N번 방까지 최소 개수의 방을 지나서 갈 때 몇 개의 방을 지나가는지(시작과 끝을 포함하여)를 계산하는 프로그램을 작성하시오. 예를 들면, 13까지는 3개, 58까지는 5개를 지난다.

---

## 입력

---

첫째 줄에 N(1 ≤ N ≤ 1,000,000,000)이 주어진다.

---

## 출력

---

입력으로 주어진 방까지 최소 개수의 방을 지나서 갈 때 몇 개의 방을 지나는지 출력한다.

---

<img width="434" alt="image" src="https://user-images.githubusercontent.com/82592845/166928233-d7d403ed-9ff4-4eb8-b836-b1bbf1bddef1.png">

## 풀이과정

---

1. 입력을 받는다.
2. for문은 조건문을 어떻게 써야할지 몰라서 while 을 썼다.
3. while 문의 종료 조건은 입력값보다 중심값이 같거나 클때이다.

   중심값 += 6\*i 를해서 값을 알아낸다.

```jsx
const input = require("fs")
  .readFileSync("/dev/stdin")
  .toString()
  .trim()
  .split(" ");

let put = parseInt(input[0]);
let center = 1;
let a = 6;

let i = 0;
while (true) {
  i++;
  if (put <= center) {
    console.log(i);
    break;
  }
  center += a * i;
}
```
