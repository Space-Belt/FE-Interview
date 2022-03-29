## JavaScript 배열!

---

### 사용할 데이터

```
const inventors = [
      { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
      { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
      { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
      { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
      { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
      { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
      { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
      { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
      { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
      { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
      { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
      { first: 'Hanna', last: 'Hammarström', year: 1829, passed: 1909 }
    ];

    const people = [
      'Bernhard, Sandra', 'Bethea, Erin', 'Becker, Carl', 'Bentsen, Lloyd', 'Beckett, Samuel', 'Blake, William', 'Berger, Ric', 'Beddoes, Mick', 'Beethoven, Ludwig',
      'Belloc, Hilaire', 'Begin, Menachem', 'Bellow, Saul', 'Benchley, Robert', 'Blair, Robert', 'Benenson, Peter', 'Benjamin, Walter', 'Berlin, Irving',
      'Benn, Tony', 'Benson, Leana', 'Bent, Silas', 'Berle, Milton', 'Berry, Halle', 'Biko, Steve', 'Beck, Glenn', 'Bergman, Ingmar', 'Black, Elk', 'Berio, Luciano',
      'Berne, Eric', 'Berra, Yogi', 'Berry, Wendell', 'Bevan, Aneurin', 'Ben-Gurion, David', 'Bevel, Ken', 'Biden, Joseph', 'Bennington, Chester', 'Bierce, Ambrose',
      'Billings, Josh', 'Birrell, Augustine', 'Blair, Tony', 'Beecher, Henry', 'Biondo, Frank'
    ];
```

---

### Filter

```
const fifteen = inventors.filter(function(inventor) {
    if(inventor.year >= 1500 && inventor.year < 1600) {
        return true;  // keep it!
    }
});
const fifteen = inventors.filter(inventor =>
(inventor.year >= 1500 && inventor.year < 1600));
```

- 1500 이상 1600미만
- filter는 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환한다.
- 조건으로 array의 요소들을 필터링한다.
- arr.filter(callback(element[, index[, array]])[, thisArg])

### Map

```
const fullNames = inventors.map(inventor =>
`${inventor.first} ${inventor.last}`);
console.log(fullNames);

예 ) Albert Einstein
```

- Map 메소드는 배열내의 모든 요소에 대해 주어진 함수의 return(결과값)을 모아 새로운 배열을 반환한다.
- arr.map(callback(currentValue[, index[, array]])[, thisArg])

### reduce

```
var totalYears = 0;

for (var i = 0; i < inventors.length; i++) {
    totalYears += inventors[i].year
}

const totalYears = inventors.reduce((total, inventor) => {
    return total +(inventor.passed - inventor.year);
}, 0);

console.log(totalYears);
```

- 상세 설명
  Reduce Method의 기본적인 형식이다.

  ```
  배열.reduce((누적값, 현재값, 인덱스, 요소) => {
      return 결과
  }, 초기값);
  ```

  누적값으로 연한하여 보통 덧셈으로 예시를 들지만, 덧셈 함수가 아니다.

  ```
  const totalYears = inventors.reduce((total, inventor) => {
    return total + (inventor.passed - inventor.year);
  }, 0);
  ```

  `inventor` 요소의 `passed`와 `year` 값의 차를 `total`에 누적해서 더한다. `total`은 , `}, 0);`에서 0으로 누적값을 선언하고 초기화 한다.

### Sort

```html
const ordered = inventors.sort(function(a, b) { if(a.year > b.year) { return 1;
} else { return -1; } }); const ordered = inventors.sort((a, b) => a.year >
b.year ? -1 : 1); console.table(ordered);
```

## sort

- 배열의 요소를 정렬하여 반환한다.
- default 순서는 유니코드 기준이다.
-

- 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다
- 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다.
- a.year > b.year -1, 1 값 이용하여 정렬
