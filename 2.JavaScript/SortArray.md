> <h2>목표!!!</h2>
>
> 배열을 알파벳순으로 정렬하여 화면에 표시하기 (관사 제외)

<img width="1145" alt="image" src="https://user-images.githubusercontent.com/82592845/161684324-31c2a03f-3cac-47b8-9beb-b825aad1b7b9.png">

---

> HTML

```jsx
<ul id="bands"></ul>
```

- 배열을 담을 `<ul>` 생성

---

> CSS

```jsx
body {
  margin: 0;
  font-family: sans-serif;
  background: url("https://source.unsplash.com/nDqA4d5NL0k/2000x2000");
  background-size: cover;
  display: flex;
  align-items: center;
  min-height: 100vh;
}

#bands {
  list-style: inside square;
  font-size: 20px;
  background: white;
  width: 500px;
  margin: auto;
  padding: 0;
  box-shadow: 0 0 0 20px rgba(0, 0, 0, 0.05);
}

#bands li {
  border-bottom: 1px solid #efefef;
  padding: 20px;
}

#bands li:last-child {
  border-bottom: 0;
}

a {
  color: #ffc600;
  text-decoration: none;
}
```

- `list-style: inside square;` `li` 태그의 스타일 속성
- `li:last-child` `li` 태그중 마지막 자식요소

> JavaScript

1. 사용할 배열

```jsx
const bands = [
  "The Plot in You",
  "The Devil Wears Prada",
  "Pierce the Veil",
  "Norma Jean",
  "The Bled",
  "Say Anything",
  "The Midway State",
  "We Came as Romans",
  "Counterparts",
  "Oh, Sleeper",
  "A Skylit Drive",
  "Anywhere But Here",
  "An Old Dog",
];
```

---

2. a / an / the 같은 관사를 제외하고 알파벳순으로 정렬해야하기 때문에 관사 없애줄 함수

```jsx
function strip(bandName) {
  return bandName.replace(/^(a |the |an )/i, "").trim();
}
```

- `replace(바꾸어질것, 바꿀것)`
- `.trim(); 앞, 뒤의 공백 제거`

---

3. 알파벳순으로 정렬해주기(변경전)

```jsx
const sortedBands = bands.sort(function (a, b) {
  // return strip(a) > strip(b) ? 1 : -1; 아래와 동일
  if (strip(a) > strip(b)) {
    return 1;
  } else {
    return -1;
  }
});
```

- 이렇게 하나의 일만 할때에는 function , return , {} 를 없애고 아래처럼 하자 ( 변경 후 )

```jsx
const sortedBands = bands.sort((a, b) => (strip(a) > strip(b) ? 1 : -1));
```

---

4. `<ul>` 안에 정렬된 배열 `<li>` 로 삽입하기

```jsx
document.querySelector("#bands").innerHTML = sortedBands
  .map((band) => `<li>${band}</li>`)
  .join("");
```

- innerHTML 으로 삽입해준다.
- 전에도 봤듯이 innerHTML을 사용할때 `.join(””)` 이 없으면
