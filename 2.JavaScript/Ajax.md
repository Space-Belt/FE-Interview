# Ajax, regex

<img alt="image" src="https://user-images.githubusercontent.com/82592845/163531266-31d0205b-ee1c-442e-930f-218008cd2047.png">

> <h2>목표</h2>

- API 데이터 사용하기
- Regex 정규표현식으로 데이터 찾기

---

> <h3> 자바스크립트 </h3>

### 1. API 주소 변수에 담기

```jsx
const endpoint =
  "https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json";
```

---

### 2. 데이터를 받아 배열에 넣어주기

```jsx
fetch(endpoint)
  .then((blob) => blob.json())
  .then((data) => cities.push(...data));
```

- fetch() 는 비동기 네트워크 통신을 하는 메소드며, Promise 객체를 반환한다.
- 하지만 `fetch(endpoint)` 이렇게만 진행한다면 데이터를 표시할 수 없다. 그러므로 JSON 데이터를 받아오기 위해 `blob => blob.json()` 과정을 진행하면 아래와 같은 데이터를 확인 할 수 있다.

  <img width="354" height= "200" alt="image" src="https://user-images.githubusercontent.com/82592845/163534502-f673410b-3787-4709-babd-c6dc8ad6a93c.png">

- 그 후 받은 데이터들을 cities 배열에 push해준다.
- [Blob Mdn](https://developer.mozilla.org/ko/docs/Web/API/Blob)
- [Blob 정리해놓은곳](https://heropy.blog/2019/02/28/blob/)

### 3. 검색 하는 함수 생성( filter, RegExp 정규 표현식 사용)

```jsx
function findMatches(wordToMatch, cities) {
  return cities.filter((place) => {
    const regex = new RegExp(wordToMatch, "gi");
    return place.city.match(regex) || place.state.match(regex);
  });
}
```

- cities에서 검색된 결과를 return 해주는 함수를 생성한다.
- regex는 정규 표현식, 또는 정규식은 문자열에서 특정 문자 조합을 찾기 위한 패턴이다.
- gi 란 g(전역에서 탐색), i(대소문자를 구분하지 않음)이다.
- 즉 이 함수는 검색어를 city와 state에서 찾아 리턴해주는것이다.

### 4. 화면에 보여지게 하기

```jsx
function displayMatches() {
  const matchArray = findMatches(this.value, cities);
  const html = matchArray
    .map((place) => {
      const regex = new RegExp(this.value, "gi");
      const cityName = place.city.replace(
        regex,
        `<span class="hl">${this.value}</span>`
      );
      const stateName = place.state.replace(
        regex,
        `<span class="hl">${this.value}</span>`
      );
      return `
  <li>
      <span class="name">${cityName}, ${stateName}</span>
      <span class="population">${numberWithCommas(place.population)}</span>
  </li>
`;
    })
    .join("");
  suggestions.innerHTML = html;
}
```

- matchArray 변수에 findMatches함수의 결과를 담는다.
- 그 후 matchArray를 mapping 하는데 mapping에 조건이 존재한다.
- 검색어를 regex 변수에 담아서 hl class와 replace를 사용하여 브라우저에 보일때 하이라이트가 되도록 했다. (맨 위 사진처럼 검색어부분에 하이라이트를 준것이다.)
- 다음으로 li태그에 변경된 도시이름과 주이름을 담고 인구수를 화면에 표시한다.
- 이때 숫자의 가독성의 높이기 위해 아래의 정규표현식을 사용한다.

```jsx
function numberWithCommas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
}
```

- 3자리마다 `,` 가 들어가도록 해준다.
