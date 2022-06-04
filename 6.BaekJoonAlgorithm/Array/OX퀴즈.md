# OX 퀴즈

## 문제

---

"OOXXOXXOOO"와 같은 OX퀴즈의 결과가 있다. O는 문제를 맞은 것이고, X는 문제를 틀린 것이다. 문제를 맞은 경우 그 문제의 점수는 그 문제까지 연속된 O의 개수가 된다. 예를 들어, 10번 문제의 점수는 3이 된다.

"OOXXOXXOOO"의 점수는 1+2+0+0+1+0+0+1+2+3 = 10점이다.

OX퀴즈의 결과가 주어졌을 때, 점수를 구하는 프로그램을 작성하시오.

---

## 입력

---

첫째 줄에 테스트 케이스의 개수가 주어진다. 각 테스트 케이스는 한 줄로 이루어져 있고, 길이가 0보다 크고 80보다 작은 문자열이 주어진다. 문자열은 O와 X만으로 이루어져 있다.

---

## 출력

---

각 테스트 케이스마다 점수를 출력한다.

---

<img width="708" alt="image" src="https://user-images.githubusercontent.com/82592845/171988727-fc1a02ea-4e65-49ca-9fb4-2a00cb48fa55.png">

## 로직

---

1. 입력을 받는다. 총 input[5][n] 까지 있다.
2. count 와 sum 을 준비한다.
3. 2중 for문과 if문을 사용해서 배열을 차례대로 O X 를 구별해서 O가 나오면
   count가 증가하게 하여 sum에 더하고 X가 나오면 count를 0으로 초기화시킨다.
4. sum을 출력하면 몇점인지 나온다.

---

## 풀이

---

```jsx
let input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

let arrayCount = Number(input[0]);

let count = 0;
let sum = 0;
for (let i = 1; i <= arrayCount; i++) {
  count = 0;
  sum = 0;
  for (let j = 0; j < input[i].length; j++) {
    if (input[i][j] === "O") {
      count++;
    } else {
      count = 0;
    }

    sum += count;
  }

  console.log(sum);
}
```
