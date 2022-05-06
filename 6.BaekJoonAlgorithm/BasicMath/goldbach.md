# 골드바흐

## 문제

---

1보다 큰 자연수 중에서  1과 자기 자신을 제외한 약수가 없는 자연수를 소수라고 한다. 예를 들어, 5는 1과 5를 제외한 약수가 없기 때문에 소수이다. 하지만, 6은 6 = 2 × 3 이기 때문에 소수가 아니다.

골드바흐의 추측은 유명한 정수론의 미해결 문제로, 2보다 큰 모든 짝수는 두 소수의 합으로 나타낼 수 있다는 것이다. 이러한 수를 골드바흐 수라고 한다. 또, 짝수를 두 소수의 합으로 나타내는 표현을 그 수의 골드바흐 파티션이라고 한다. 예를 들면, 4 = 2 + 2, 6 = 3 + 3, 8 = 3 + 5, 10 = 5 + 5, 12 = 5 + 7, 14 = 3 + 11, 14 = 7 + 7이다. 10000보다 작거나 같은 모든 짝수 n에 대한 골드바흐 파티션은 존재한다.

2보다 큰 짝수 n이 주어졌을 때, n의 골드바흐 파티션을 출력하는 프로그램을 작성하시오. 만약 가능한 n의 골드바흐 파티션이 여러 가지인 경우에는 두 소수의 차이가 가장 작은 것을 출력한다.

---

## 입력

---

첫째 줄에 테스트 케이스의 개수 T가 주어진다. 각 테스트 케이스는 한 줄로 이루어져 있고 짝수 n이 주어진다.

---

## 출력

---

각 테스트 케이스에 대해서 주어진 n의 골드바흐 파티션을 출력한다. 출력하는 소수는 작은 것부터 먼저 출력하며, 공백으로 구분한다.

---

## 제한

---

4 ≤ n ≤ 10,000

---

<img width="442" alt="image" src="https://user-images.githubusercontent.com/82592845/167099918-1b33257a-40cb-4904-9f75-c7d878e0db49.png">

## 풀이

---

```html
let input = [3, 8, 10, 16]; let testCount = input.shift(); //
console.log(testCount); for (let i = 0; i < testCount; i++) {
findDecimal(input[i]); } function findDecimal(num) { let testCase = num; let
sumIsResult = []; let decimalArr = []; let lastTestArr = []; //
console.log("here is first " + decimalArr); for (let i = 2; i <= testCase; i++)
{ if (i === 2) { decimalArr.push(i); continue; } for (let j = 2; j < testCase;
j++) { if (i % j === 0) { break; } if (j * j > i) { decimalArr.push(i); break; }
} } // console.log(num + " + " + decimalArr); // console.log(decimalArr); for
(let i = 0; i < decimalArr.length; i++) { for (let j = 0; j < decimalArr.length;
j++) { if (decimalArr[i] + decimalArr[j] === testCase) { // if (decimalArr[i] -
[j] >= 0) { sumIsResult.push(decimalArr[i], decimalArr[j]); decimalArr[i] = 0;
// } } } } // console.log(sumIsResult); if (sumIsResult.length === 2) {
console.log(sumIsResult.join(" ")); } else { for (let i = 1; i <
sumIsResult.length; i += 2) { lastTestArr.push(sumIsResult[i]); } let min =
Math.min.apply(null, lastTestArr); // console.log(min); //
console.log(sumIsResult.indexOf(min)) if (sumIsResult.indexOf(min) ===
sumIsResult.lastIndexOf(min)) { console.log(
sumIsResult[sumIsResult.indexOf(min) - 1] + " " +
sumIsResult[sumIsResult.indexOf(min)] ); } else { console.log(
sumIsResult[sumIsResult.indexOf(min)] + " " +
sumIsResult[sumIsResult.lastIndexOf(min)] ); } } // console.log(lastTestArr); }
```

- 시간복잡도가 너무 안좋다.
- 아래 비슷한 풀이

```html
let input = [3, 8, 10, 16]; const C = input.shift(); for (let i = 0; i < C; i++)
{ const n = input.shift(); const isPrimeNumArr = Array(n + 1).fill(true); const
primeNumArr = []; const checkPrimePair = []; isPrimeNumArr.fill(true);
isPrimeNumArr[0] = isPrimeNumArr[1] = false; for (let j = 2; j <= n; j++) { if
(Math.pow(j, 2) > 1000000) { break; } else { for (let squared = Math.pow(j, 2);
squared <= n; squared += j) { isPrimeNumArr[squared] = false; } } } for (let i =
2; i <= n; i++) { if (isPrimeNumArr[i]) { primeNumArr.push(i); } }
console.log(primeNumArr); for (let i = 0; i < primeNumArr.length; i++) { for
(let j = i; j < primeNumArr.length; j++) { if (n - primeNumArr[i] -
primeNumArr[j] === 0) { checkPrimePair.push([primeNumArr[i]]); // 여기서
이런식으로 이차원 배열로 만들었다. checkPrimePair[checkPrimePair.length -
1].push(primeNumArr[j]); console.log( "this is i " + primeNumArr[i] + " this is
j " + primeNumArr[j] ); } } } console.log(checkPrimePair); let min =
checkPrimePair[0][1] - checkPrimePair[0][0]; let indexOfMin = 0; for(let i = 0;
i < checkPrimePair.length; i++) { if(checkPrimePair[i][1] - checkPrimePair[i][0]
< min) { min = checkPrimePair[i][1] - checkPrimePair[i][0]; indexOfMin = i; } }
console.log(checkPrimePair[indexOfMin].join(' ')); }
```

- function을 사용하지 않고 for문안에서 모든것을 활용했다.
- 이차원배열을 저런식으로 넣었다.
- min에 일단 값을 처음것으로 넣어주고 다음것이 더작으면 바꾸는 형식
- 그리고 인덱스 넘버를 저장해서 출력!

### 간단한 풀이

```html
let input =
require('fs').readFileSync('/dev/stdin').toString().trim().split('\n').map(a=>+a);
input.shift(); const isPrime = (num) => { if (num === 1 || num === 0) { return
false; } for (let i = 2; i * i <= num; i++) { if (num % i === 0) { return false;
} } return true; }; input.forEach((e) => { let prime = 0; for (let i = 1; i < e
/ 2 + 1; i++) { if (isPrime(i) && isPrime(e - i)) { prime = i; } }
console.log(prime, e - prime); });
```

- i라는 숫자의 합이 j = 1 j++ ; j + (i-j) 인것을 생각해서 풀었다.
- i , j 숫자 둘다 소수인것을 확인시키고 true면 출력 시킨다.

### 다른풀이

```html
/** * let targetNumber = 입력받은 수 / 2; // 짝수니까 * (targetNumber - 1)과
(targetNumber + 1)이 모두 소수인지 확인을 반복 */ const readline =
require("readline"); const rl = readline.createInterface({ input: process.stdin,
output: process.stdout, }); const input = []; const getPrimeNumber = target => {
const array = Array(target) .fill() .map(() => false); // 1 제외 array[0] =
true; // 2의 배수 제외 for (let i = 3; i < target; i += 2) { array[i] = true; }
// 홀수의 배수들 제외 ( 짝수는 이미 제외했으므로 i += 2 ) for (let i = 2; i <
target; i += 2) { // 이미 제외된놈은 연산 X if (array[i]) continue; // 현재
숫자의 배수 제외 ( 반복은 100에 가까울 때 까지 ) for (let j = 1; j <=
Math.floor(target / (i + 1)); j += 2) { // 본인 제외 ( 3, 5, 7, 11 등 ) if (j
=== 1) continue; // 본인의 배수 제외 ( 9, 15, 21 등... 짝수는 애초에 연산을 안함
) array[(i + 1) * j - 1] = true; } } return array; }; rl.on("line", line => {
input.push(+line); if (input.length >= 2 && input.length - 1 === +input[0])
rl.close(); }).on("close", () => { input.shift(); // 입력한 수중에 최댓값 구하기
const max = input.reduce((prev, curr) => (prev > curr ? prev : curr)); // 소수
구하기 ( false가 소수, array[0]이 숫자 1 ) const primeNumberList =
getPrimeNumber(max); // 정답 기록 let answer = ""; input.forEach(n => { //
짝수라서 /2 // +1, -1은 while문 제일 앞에 += 1, -=1 적기 위해서임 ( 이게 더 보기
좋아서 ) let biggerNumber = n / 2 - 1; let smallerNumber = n / 2 + 1; let
condition = true; while (condition) { biggerNumber += 1; smallerNumber -= 1; if
(!primeNumberList[biggerNumber - 1] && !primeNumberList[smallerNumber - 1]) {
answer += `${smallerNumber} ${biggerNumber}\n`; condition = false; } } });
console.log(answer); process.exit(); });
```

- 입력숫자중 가장 큰 숫자를 이용해서 소소를 다 구해서 배열에 집어넣는다.
- 배열의 인덱스 넘버를 이용해서 제일 가운데 쪽 숫자부터 시작해서 차례대로 하다가 걸리면 제일 가까운 소수가 나온다.
- 이렇게는 생각을 어떻게 하는지 궁금하다.
