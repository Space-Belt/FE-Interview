## 학습 목표 !

> <h2>some, every, find, findIndex 의 사용법을 익힌다.</h2>

## 1. 나이가 19살 이상인 사람이 적어도 한명 있느냐

```jsx
// Some and Every Checks // Array.prototype.some() // is at least one person 19
or older? /* 19살 이상 있는지 확인 */ // const isAdult =
people.some(function(person) { // const currentYear = (new
Date()).getFullYear(); // if(currentYear - person.year >= 19) { // return true;
// } // }); isAdult 에 people.some 을 이용한다. // 축약 // const isAdult =
people.some(person => { // const currentYear = ; // return ((new
Date()).getFullYear() - person.year >= 19;) // }); // 다시 축약 const isAdult =
people.some(person => ((new Date()). getFullYear()) - person.year >= 19);
console.log({isAdult});
```

> some() 메서드는 배열 안의 어떤 요소라도 주어진 판별 함수를 통과하는지 테스트합니다.

> 즉, 특정 조건이 넘는 인자를 발견하면, **즉시 멈추고** `true`를 반환하게 돼요.

```jsx
function isBiggerThan10(element, index, array) {
  return element > 10;
}

[2, 5, 8, 1, 4].some(isBiggerThan10); // false
[12, 5, 8, 1, 4].some(isBiggerThan10); // true
```

두 개의 배열에 대해서 `isBiggerThan10` 함수를 인자로 넘겨주어 판단을 하게 되는 구조다. 혹은, 화살표 함수로도 가능하다.

```jsx
[2, 5, 8, 1, 4].some((elem) => elem > 10); // false
[12, 5, 8, 1, 4].some((elem) => elem > 10); // true
```

2번째 구문에서 `12` 를 발견한 즉식 `true` 를 반환하는게 특징인 듯

따라서, 1번 문제는 이렇게 해결했다.

```jsx
const current = new Date();
const currentYear = current.getFullYear();

const someFunc = people.some((person) => currentYear - person.year >= 19);
```

`new Date()` 로 현재 날짜 객체를 생성하여 `getFullYear()` 함수로 현재 년도를 추출한다.

그 결과로 `true` 를 반환한다.

## 2. 전부가 19살 이상인가?

> Array.prototype.every();

> every() 메서드는 배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트

```jsx
const allAdults = people.every(
  (person) => new Date().getFullYear() - person.year >= 19
);
console.log({ allAdults });
```

사용 구문은 `some` 과 똑같아요.

```jsx
arr.every(callback[, thisArg])
```

모든 원소에 대해서 `callback` 함수를 시행시켜보는 모습을 볼 수 있습니다.

```jsx
[12, 5, 8, 130, 44].every((elem) => elem >= 10); // false
[12, 54, 18, 130, 44].every((elem) => elem >= 10); // true
```

`some` 과 아예 같다. 다만 모든 원소가 해당 조건을 만족시켜야 `true`를 반환하는 것 만 다른걸 확인할 수 있다.

따라서 이 문제는 이렇게 간단하게 해결.

```jsx
const everyFunc = people.every((person) => currentYear - person.year >= 19);
```

결과는 `false` 를 반환하게 됨.

## 3. ID가 823423인

> comments 배열에서 ID 값이 823423 인 친구를 찾아라

> Array.prototype.find() 사용

```jsx
// const comment = comments.find(function(comment) { // if(comment.id ===
823423) { // return true; // } // }); const comment = comments.find(comment =>
(comment.id === 823423)); console.log(comment);
```

> find() 메서드는 주어진 판별 함수를 만족하는 첫 번째 요소의 값을 반환한다. 그런 요소가 없다면 undefined를 반환

사용 구문은 모두 같다

```jsx
arr.find(callback[, thisArg])
```

`callback` 을 만족시키는 **첫 번째** 원소를 반환해요.

```jsx
var inventory = [
  { name: "apples", quantity: 2 },
  { name: "bananas", quantity: 0 },
  { name: "cherries", quantity: 5 },
];

function findCherries(fruit) {
  return fruit.name === "cherries";
}

console.log(inventory.find(findCherries)); // { name: 'cherries', quantity: 5 }
```

`findCherris` 함수에서 조건을 만족하는 객체를 반환해주는 형태입니다.

따라서 이 문제는 이렇게 해결했어요.

```jsx
const findFunc = comments.find((comment) => comment.id == 823423);
```

## 04. ID 823423을 찾아 지워라

### 1. 일단 findIndex를 사용하여 인덱스를 반환한다.

- `find` 는 조건을 만족하는 객체를 반환하는 것

> findIndex() 메서드는 주어진 판별 함수를 만족하는 배열의 첫 번째 요소에 대한 인덱스를 반환합니다. 만족하는 요소가 없으면 -1을 반환합니다.

조건을 만족하는 **첫 번째 인덱스**! 이게 핵심

사용되는 문법은 같다. 그냥 인덱스를 반환하는것.

```jsx
const findIndexFunc = comments.findIndex((comment) => comment.id == 823423);
console.log(findIndexFunc); // 1 출력!     [ 0, 1, 2, 3, 4] 인덱스 순
```

### 2. 인덱스를 찾았으니 slice 또는 splice를 이용해서 인덱스를 없애보자.

> Splice를 먼저 알아보자 !

> splice() 메서드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.

```jsx
let arr = ["I", "study", "JavaScript", "right", "now"];

// 처음(0) 세 개(3)의 요소를 지우고, 이 자리를 다른 요소로 대체합니다.
arr.splice(0, 3, "Let's", "dance");

alert(arr); // now ["Let's", "dance", "right", "now"]
```

첫 번째 인자는 삭제를 시작할 index, 두 번째 인자는 갯수를 적어요.3번째 ~ 는 넣을 원소를 적을 수도 있고, 없다면 그냥 삭제만 하는 기능입니다.

```jsx
let arr = ["I", "study", "JavaScript"];

arr.splice(1, 1); // 인덱스 1부터 요소 한 개를 제거

alert(arr); // ["I", "JavaScript"]
```

문제의 정답

```jsx
comments.splice(findIndexFunc, 1);
console.table(comments);
```

좀 더 자세히 아려면

[https://im-developer.tistory.com/103](https://im-developer.tistory.com/103) 참고

---

> slice를 알아보자!

slice() 메소드는 begin부터 end 전까지의 복사본을 새로운 배열 객체로 반환한다. 즉, 원본 배열은 수정되지 않는다.

> **`slice(start[, end])`**

**`start`:** **추출 시작점에 대한 인덱스.**

- **undefined**인 경우: 0부터 slice
- **`음수`**를 지정한 경우: 배열의 끝에서부터의 길이를 나타낸다. slice(-2)를 하면 배열의 마지막 2개의 요소를 추출한다.
- **`배열의 길이와 같거나 큰 수`**를 지정한 경우: 빈 배열을 반환한다.

**`end:** **추출을 종료할 기준 인덱스.**` (end를 제외하고 그 전까지의 요소만 추출한다.)

- **지정하지 않을 경우**: 배열의 끝까지 slice
- **음수**를 지정한 경우: 배열의 끝에서부터의 길이를 나타낸다. slice(2, -1)를 하면 세번째부터 끝에서 두번째 요소까지 추출
- **배열의 길이와 같거나 큰 수**를 지정한 경우: 배열의 끝까지 추출.

**반환값:** **추출한 요소를 포함한 새로운 배열.**

```jsx
var arr = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]; var arr1 = arr.slice(3, 5); // [4,
5] var arr2 = arr.slice(undefined, 5); // [1, 2, 3, 4, 5] var arr3 =
arr.slice(-3); // [8, 9, 10] 뒤에서 3칸 var arr4 = arr.slice(-3, 9); // [8, 9]
var arr5 = arr.slice(10); // [] 시작 인덱스가 10이면 끝이 11 (부터 전까지임) var
arr6 = arr.slice(4); // [5, 6, 7, 8, 9, 10] 끝을 지정하지 않으면 끝까지 포함 var
arr9 = arr.slice(2, 15); // [3, 4, 5, 6, 7, 8, 9, 10]
```

```jsx
const newComments = [ ...comments.slice(0, index), ...comments.slice(index + 1)
]; ...comments.slice(0, 1) 으로 { text: 'Love this!', id: 523423 }
...comments.slice(index + 1) { text: 'You are the best', id: 2039842 }, { text:
'Ramen is my fav food ever', id: 123523 }, { text: 'Nice Nice Nice!', id: 542328
} ...이 없으면 저 배열에 인덱스번호 0 에 { text: 'Love this!', id: 523423 }
인덱스번호 1 에 { text: 'You are the best', id: 2039842 }, { text: 'Ramen is my
fav food ever', id: 123523 }, { text: 'Nice Nice Nice!', id: 542328 } 가
들어간다. ( 어레이 안의 어레이가 됨 ) ...을 사용해서 하나하나 풀어준다.
```
