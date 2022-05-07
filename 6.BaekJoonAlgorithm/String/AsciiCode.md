# 아스키 코드

## 문제

---

알파벳 소문자, 대문자, 숫자 0-9중 하나가 주어졌을 때, 주어진 글자의 아스키 코드값을 출력하는 프로그램을 작성하시오.

---

## 입력

---

알파벳 소문자, 대문자, 숫자 0-9 중 하나가 첫째 줄에 주어진다.

---

## 출력

---

입력으로 주어진 글자의 아스키 코드 값을 출력한다.

---

<img width="447" alt="image" src="https://user-images.githubusercontent.com/82592845/167245358-5345dc64-7da5-4725-899f-9e906b7837f4.png">

## 로직

---

1. 입력을 받는다.
2. (문자열).charCodeAt(인덱스값) 으로 아스키코드 출력

---

## 풀이

---

1. 헤매서 푼 방법

```jsx
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});
let input = [];
rl.on('line', function(line){
    input.push(line);
    let a = input[0];
    console.log(a.charCodeAt());
}).on('close', function() {
    process.exit();
})

======= 문자열로도 가능 ============

const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});
let input = '';
rl.on('line', function(line){
    input = line;
    let a = input;
    console.log(a.charCodeAt());
}).on('close', function() {
    process.exit();
})
```

- readline에서 그냥 배열사용하지 않고 하려 했더니 레퍼런스 오류가 계속 나왔었다.

1. fs로 쉽게 풀이

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString();
const a = input;
console.log(a.charCodeAt());
```

# 알아 놓기!!

"문자열".charCodeAt([문자열 자릿수]);

- 예제 1

"charCodeAt" 이라는 문자열의 "C"를 아스키 코드로 변환하시오.

- 예제 1 소스

```jsx
var val = "charCodeAt";

document.write("val.charCodeAt(4) : " + val.charCodeAt(4) + "<br>");
```

결과

---

```jsx
val.charCodeAt(4) : 67
```

---

- 자바스크립트 fromCharCode 사용방법 (아스키코드를 문자열로 변환)

String.fromCharCode([아스키코드값]);

- 예제 2

아스키 코드값이 111에 해당하는 문자를 출력하시오.

- 예제 2 소스

```jsx
var val = 111;

document.write(
  "String.fromCharCode(val) : " + String.fromCharCode(val) + "<br>"
);
```

결과

---

`String.fromCharCode(val) : o`

---

- 예제 3

`String.fromCharCode`를 이용하여 문자열 "ASCII" 를 출력하시오.

- 예제 3 소스

`document.write("String.fromCharCode(65, 83, 67, 73, 73) : " + String.fromCharCode(65, 83, 67, 73, 73) + "<br>");`

결과

---

`String.fromCharCode(65, 83, 67, 73, 73) : ASCII`

---
