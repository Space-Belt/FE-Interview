# 설탕 배달

## 문제

---

상근이는 요즘 설탕공장에서 설탕을 배달하고 있다. 상근이는 지금 사탕가게에 설탕을 정확하게 N킬로그램을 배달해야 한다. 설탕공장에서 만드는 설탕은 봉지에 담겨져 있다. 봉지는 3킬로그램 봉지와 5킬로그램 봉지가 있다.

상근이는 귀찮기 때문에, 최대한 적은 봉지를 들고 가려고 한다. 예를 들어, 18킬로그램 설탕을 배달해야 할 때, 3킬로그램 봉지 6개를 가져가도 되지만, 5킬로그램 3개와 3킬로그램 1개를 배달하면, 더 적은 개수의 봉지를 배달할 수 있다.

상근이가 설탕을 정확하게 N킬로그램 배달해야 할 때, 봉지 몇 개를 가져가면 되는지 그 수를 구하는 프로그램을 작성하시오.

---

## 입력

---

첫째 줄에 N이 주어진다. (3 ≤ N ≤ 5000)

---

## 출력

---

상근이가 배달하는 봉지의 최소 개수를 출력한다. 만약, 정확하게 N킬로그램을 만들 수 없다면 -1을 출력한다.

<img width="712" alt="image" src="https://user-images.githubusercontent.com/82592845/166136652-e816bb4b-6f61-4f85-ad29-7b11564602a5.png">

## 느낀점

---

뇌를 좀더 잘 사용하자.

---

## 풀이

---

```jsx
let input = require("fs").readFileSync("/dev/stdin");

const a = 5;
const b = 3;
let count = 0;
while (input > 0) {
  if (input % a === 0) {
    input -= a;
    count++;
  } else if (input % b === 0) {
    input -= b;
    count++;
  } else if (input > a) {
    input -= a;
    count++;
  } else {
    count = -1;
    break;
  }
}
console.log(count);
```

- 5를 뺴는게 우선이다. 숫자가 5를 넘어가면 5를 먼저 빼주고 카운트를 올린다.
- 그리고 if문에 걸린다면 거기서도 그 수만큼 빼고 카운트를 더해준다.
- 그러면 결과가 나온다.

```jsx
let input = require("fs").readFileSync("/dev/stdin");

const a = 5;
const b = 3;
let count = 0;
while (true) {
  if (input % 5 === 0) {
    console.log(input / 5 + count);
    break;
  }
  if (input < 0) {
    console.log(-1);
    break;
  }
  count++;
  input -= 3;
}
```

- 5의 배수라면 그냥 5로 나눈 몫으로 하면 된다. 그러니 base는 -3을 해보고 진행한다.
- 5로 나눠진다면 나눠진 몫과 더해진 count를 더한다.
- 일단 카운트를 올리고 인풋에 -3을 해준다.
- input이 0 보다 작아진다면 그만 한다.
