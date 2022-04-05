# 배열 비교

<img width="728" alt="image" src="https://user-images.githubusercontent.com/82592845/161693482-57eedc93-1648-41ed-af5d-243ceb3def17.png">
<img width="689" alt="image" src="https://user-images.githubusercontent.com/82592845/161693552-8520f39e-4021-40b5-a287-dfe3435b0f42.png">
<img width="581" alt="image" src="https://user-images.githubusercontent.com/82592845/161693612-3ec76a26-d4a2-44c8-a60c-9f9ed02f2d98.png">

---

> <h2>풀이</h2>

1. 배열을 길이가 다르면 애초에 성립하지 않기에 if문으로 false를 return한다.

```
if(first.length !== second.length) {
  return false;
}
```

2. 알파벳(string)들을 객체로 만든다. ex) "aaabbc" => {a: 3 b: 2 c: 1}

```
const check1 = {};
const check2 = {};
for (const a of first) {
    check1[a] = (check1[a] || 0) + 1;
}
for (const b of second) {
    check2[b] = (check2[b] || 0) + 1;
}
```

3. for문 객체를 차례로 순환시키는 for ...in을 사용하여 비교한다.

```
for(const abc in check1) {
    if(check1[abc] !== check2[abc]) {
        return false;
    }
}
```

4. 위의 조건들을 다 통과한다면 true ! (완성)

```
function checkAA(aa, bb) {
    if(first.length !== second.length) {
      return false;
    }
    const check1 = {};
    const check2 = {};
    for (const a of first) {
        check1[a] = (check1[a] || 0) + 1;
    }
    for (const b of second) {
        check2[b] = (check2[b] || 0) + 1;
    }
    for(const abc in check1) {
        if(check1[abc] !== check2[abc]) {
            return false;
        }
    }
    return true;
}
```

---

> <h2>풀이 개선</h2>

```
function valid (first, second) {
    if(first.length !== second.length) {
        return false;
    }
    const check = {};
    for(let val of first) {
        check[val] = (check[val] || 0) + 1
    }
    for(let i = 0; i < second.length; i++) {
        let letter = second[i];
        if(!check[letter]) {
            return false;
        } else {
            check[letter] -= 1;
        }
    }
    return true;
}

```

- 처음 매개변수길이비교는 같다.
- 첫 풀이는 두 스트링을 둘다 객체형식으로 만들었지만 first하나만 한다.
- 객체로 만들지 않은 스트링의 길이만큼 for문을 돌린다.
- `let letter = second[i]` 로 선언하여 각 글자를 한번씩 돌아가게 하고
- 그 글자가 first안에 없다면 false를 반환하고 그렇지 않은경우는 값을 -1 해주고 계속 진행하게 한다.
