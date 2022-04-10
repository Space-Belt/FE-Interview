# 배열 복사

> 목표!

> 배열의 복사, 참조에 대해서 알아보자!

```html
let age = 100; let age2 = age; console.log(age, age2); age = 200;
console.log(age, age2); let name = 'Wes'; let name2 = name; console.log(name,
name2); name = 'wesley'; console.log(name, name2);
```

### 1. 특징

- 첫 console.log(age, age2); 100 100 출력
- 두번쨰 console.log(age, age2); 200 100 출력
- string 값도 똑같이 변한다.

<aside>
💡 배열은 어떻게 변하는지 알아보자!

</aside>

### players.slice();를 이용해서 복사한것과의 차이

```html
const team2 = team; team2[2] = 'hyun'; console.log(team, team2, players); const
team2 = players.slice(); team2[2] = 'hyun';
```

- slice를 이용해 복사된 team2를 변경하면 team2 만 변경되어있다.

---

```html
const team3 = [].concat(players); console.log(team3); // 변화 없음
```

### Concat() 이란??

**`concat()`** 메서드는 인자로 주어진 배열이나 값들을 기존 배열에 합쳐서 새 배열을 반환합니다.

- 기존배열을 변경하지 않습니다.
- 추가된 새로운 배열을 반환합니다.

ex )

<aside>
💡 const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2);

</aside>

<aside>
💡 console.log(array3);
// expected output: Array ["a", "b", "c", "d", "e", "f"]

</aside>

---

## 복사

# Array.from()

**`Array.from()`** 메서드는 유사 배열 객체(array-like object)나 반복 가능한 객체(iterable object)를 얕게 복사해 새로운`Array` 객체를 만듭니다.

# Object.assign()

**`Object.assign()`** 메소드는 출처 객체로부터 모든 열거할 수 있는([enumerable](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable)) 하나 이상의 속성([own properties](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty))들을 목표 객체로 복사합니다. 이는 수정된 목표 객체를 반환합니다.

```html
const target = { a: 1, b: 2 }; const source = { b: 4, c: 5 }; const
returnedTarget = Object.assign(target, source); console.log(target); // expected
output: Object { a: 1, b: 4, c: 5 } console.log(returnedTarget); // expected
output: Object { a: 1, b: 4, c: 5 }
```

# JSON.stringify()

**`JSON.stringify()`** 메서드는 JavaScript 값이나 객체를 JSON 문자열로 변환합니다. 선택적으로, `replacer`를 함수로 전달할 경우 변환 전 값을 변형할 수 있고, 배열로 전달할 경우 지정한 속성만 결과에 포함합니다.
