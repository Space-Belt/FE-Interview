<img width="908" alt="image" src="https://user-images.githubusercontent.com/82592845/169228902-25512f8a-2092-4b15-9167-46bee7a5c50d.png">

<br/>

## 문제

---

### 두개의 문자열이 주어진다. 첫번째 문자열의 각요소가 두번째 문자열에 포함되어 있다면 true 아니면 false를 반환한다. 즉**, 함수는 첫 번째 문자열의 문자 순서를 변경하지 않고 두 번째 문자열의 어딘가에 나타나는지 확인해야 한다.**

---

<br/>

## 풀이

---

```jsx
function isSubsequence(str1, str2) {
  let i = 0;
  let j = 0;
  if (!str1) return true;
  while (j < str2.length) {
    if (str2[j] === str1[i]) i++;
    if (i === str1.length) return true;
    j++;
  }
  return false;
}
```

- 문자열이 없으면 무조건 true다.
- j 가 str2.length 와 같아지면 while문을 끝낸다.
- str2[j]의 해당 인덱스 문자와 str1[i] 를 비교하여 같으면 i++
- 만약 i 가 str1.length 와 같다면 true를 반환한다. ( `i === str1.length` ⇒ 이미 str2에 다 존재한다.)
- 다르면 j++ 해준다.

```jsx
function isSubsequence(str1, str2) {
  if (!str1) return true;
  if (!str2) return false;
  if (str2[0] === str1[0]) return isSubsequence(str1.slice(1), str2.slice(1));
  return isSubsequence(str1, str2.slice(1));
}
```

- slice()를 이용해서 풀었다.
- 결국 str1이 존재하지 않으면 true를 반환하게 되고. str2가 없을경우엔 false를 반환한다.
