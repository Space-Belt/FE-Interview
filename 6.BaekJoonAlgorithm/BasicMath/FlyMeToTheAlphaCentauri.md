# Fly me to the Alpha Centauri

## 문제

---

우현이는 어린 시절, 지구 외의 다른 행성에서도 인류들이 살아갈 수 있는 미래가 오리라 믿었다. 그리고 그가 지구라는 세상에 발을 내려 놓은 지 23년이 지난 지금, 세계 최연소 ASNA 우주 비행사가 되어 새로운 세계에 발을 내려 놓는 영광의 순간을 기다리고 있다.

그가 탑승하게 될 우주선은 Alpha Centauri라는 새로운 인류의 보금자리를 개척하기 위한 대규모 생활 유지 시스템을 탑재하고 있기 때문에, 그 크기와 질량이 엄청난 이유로 최신기술력을 총 동원하여 개발한 공간이동 장치를 탑재하였다. 하지만 이 공간이동 장치는 이동 거리를 급격하게 늘릴 경우 기계에 심각한 결함이 발생하는 단점이 있어서, 이전 작동시기에 k광년을 이동하였을 때는 k-1 , k 혹은 k+1 광년만을 다시 이동할 수 있다. 예를 들어, 이 장치를 처음 작동시킬 경우 -1 , 0 , 1 광년을 이론상 이동할 수 있으나 사실상 음수 혹은 0 거리만큼의 이동은 의미가 없으므로 1 광년을 이동할 수 있으며, 그 다음에는 0 , 1 , 2 광년을 이동할 수 있는 것이다. ( 여기서 다시 2광년을 이동한다면 다음 시기엔 1, 2, 3 광년을 이동할 수 있다. )
<img alt="image" src="https://user-images.githubusercontent.com/82592845/169476070-9c2403e2-8fb7-4152-9926-9b6e05ec48e0.png">
김우현은 공간이동 장치 작동시의 에너지 소모가 크다는 점을 잘 알고 있기 때문에 x지점에서 y지점을 향해 최소한의 작동 횟수로 이동하려 한다. 하지만 y지점에 도착해서도 공간 이동장치의 안전성을 위하여 y지점에 도착하기 바로 직전의 이동거리는 반드시 1광년으로 하려 한다.

김우현을 위해 x지점부터 정확히 y지점으로 이동하는데 필요한 공간 이동 장치 작동 횟수의 최솟값을 구하는 프로그램을 작성하라.

---

## 입력

---

입력의 첫 줄에는 테스트케이스의 개수 T가 주어진다. 각각의 테스트 케이스에 대해 현재 위치 x 와 목표 위치 y 가 정수로 주어지며, x는 항상 y보다 작은 값을 갖는다. (0 ≤ x < y < 231)

---

## 출력

---

각 테스트 케이스에 대해 x지점으로부터 y지점까지 정확히 도달하는데 필요한 최소한의 공간이동 장치 작동 횟수를 출력한다.

---

<img alt="image" src="https://user-images.githubusercontent.com/82592845/169476195-8f72ea9b-76d7-4702-9f99-b7e125afd1bf.png">

## 풀이

---

```jsx
let input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

let count = parseInt(input.shift());
let i = 0;
let arr = [];
for (let i = 0; i < count; i++) {
  arr[i] = input[i].split(" ");
  let X = parseInt(arr[i][0]);
  let Y = parseInt(arr[i][1]);
  let distance = Y - X;

  let max = parseInt(Math.sqrt(distance));

  if (max === Math.sqrt(distance)) {
    console.log(max * 2 - 1);
  } else if (distance <= max * max + max) {
    console.log(max * 2);
  } else {
    console.log(max * 2 + 1);
  }
}
```

- 입력받는것은 똑같다.
- 입력받아서 차례대로 받아서 둘의 차를 distance에 넣는다.
- distance의 루트값을 소수점 내림으로 지정해준다.
- max 값과 내림하지않은 distance의 루트값이 같으면 제곱이다. 제곱인 자리에는 max \* 2 에 -1의 규칙이 보인다.
- 그외에 distance의 길이와 max*max+max 가 같거나 클때는 내림해준 max * 2 가 count값으로 보이는 규칙이 있다
- distance가 max*max+max 보다 클때는 max*2+1 이 count값이 되는 규칙이 있다.
  <img alt="image" src="https://user-images.githubusercontent.com/82592845/169476323-76fbd568-57bb-44e3-ac5d-fb8e81027769.png">
      - 1   count 1
      - 2  count 2
      - 3 ~ 4 count 3
      - 5 ~ 6 count 4  distance <= max * max + max
      - 7 ~ 9 count 5   반대
      - 10 ~ 12 count 6  distance <= max * max + max
      - 13 ~ 16 count 7  반대
      - 17 ~ 20 count 8  distance <= max * max + max
      - 18 ~ 25 count 9  반대
      - 제곱이 딱 떨어지는 숫자들은 max * 2 - 1 이 카운트값이 된다. 1 4 9 16 25 36 등등

### 다른 풀이

---

```jsx
//const input = require('fs').readFileSync('/dev/stdin').toString().trim().split(' ');

const input = ["3", "0 3", "1 5", "45 50"];

let T = parseInt(input[0]);

for (let i = 1; i <= T; i++) {
  let spl = input[i].split(" ").map((a) => parseInt(a));

  let inputGap = spl[1] - spl[0];

  let a = 0;
  let b = a + 1;
  let sum = a + b;
  let point = sum;

  while (true) {
    if (inputGap <= point) {
      if (inputGap === 1) {
        console.log(1);
        break;
      } else {
        if (inputGap <= point - b) {
          console.log(sum - 1);
          break;
        } else {
          console.log(sum);
          break;
        }
      }
    }
    a++;
    b = a + 1;
    sum = a + b;
    point += sum;
  }
}
```

- a,b,sum,point 값을 이용한다.
- point 에는 제곱인 숫자가 들어간다.

a:0 1 2 3 4 5 6

b:1 2 3 4 5 6 7

s:1 3 5 7 9 11 13

p:1 4 9 16 25 36 49

- point - b 는 숫자가 변하는 시점을 알려준다.
- sum 은 해당하는 숫자일때의 count 개수다. point -b 를 기준으로 sum - 1 또는 sum 그대로 출력한다.
- point - b 의 값은 그 숫자 다음부터 수가 변한다. 그러므로 그 거리가 숫자 보다 작다면 sum -1 을 해준다.
- inputGap 이 point - b 보다 크다면 숫자가 변했으니 sum 을 출력한다.
- a , b , sum , point 를 이용해서 이렇게

## 코드샌드박스 코드

```jsx
let input = ["3", "3 4", "1 9", "30 50"];

let count = parseInt(input.shift());
let i = 0;
let arr = [];
for (let i = 0; i < count; i++) {
  arr[i] = input[i].split(" ");
  let X = parseInt(arr[i][0]);
  let Y = parseInt(arr[i][1]);
  let distance = Y - X;

  let max = parseInt(Math.sqrt(distance));

  if (max === Math.sqrt(distance)) {
    console.log("1   " + Math.sqrt(distance));
    console.log("1   " + max);
    console.log(max * 2 - 1);
  } else if (distance <= max * max + max) {
    console.log("2   " + Math.sqrt(distance));
    console.log("2   " + max);
    console.log("2   " + distance);
    console.log(max * 2);
  } else if (max !== Math.sqrt(distance) || distance > max * max + max) {
    console.log("3   " + Math.sqrt(distance));
    console.log("3   " + max);
    console.log(max * 2 + 1);
  }
}
console.log(Math.sqrt(2)); // 2 2 3
// Math.ceil
```
