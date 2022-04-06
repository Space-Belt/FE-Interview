# Array Reduce

> <h3>HTML</h3>

```jsx
<ul class="videos">
  <li data-time="5:43">Video 1</li>
  <li data-time="2:33">Video 2</li>
  <li data-time="3:45">Video 3</li>
  <li data-time="0:47">Video 4</li>
  <li data-time="5:21">Video 5</li>
  // 생략
</ul>
```

- data-time attribute
  - data-("이름")
  - 이름은 한글자 이상이어야 하며 대문자를 포함할수 없다.
  - 속성값은 모든 문자열이 될 수 있다.
  - HTML5에서 생겼다.
  - [더 알아보기](http://html5doctor.com/html5-custom-data-attributes/)

---

> <h3>JAVASCRIPT</h3>

```
const timeNodes = Array.from(document.querySelectorAll('[data-time]'));
```

- Array.from
  - Array.from() 메서드는 배열이나 반복 가능한 객체를 얕은 복사로 새로운 배열을 만든다.
  ```jsx
  EX)
  console.log(Array.from('Woo'));  // Array ["W", "o", "o"]
  console.log(Array.from([3,4,5], x=> x+x)); // Array [6,8,10]
  ```
  - [참고](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from#map%EC%97%90%EC%84%9C_%EB%B0%B0%EC%97%B4_%EB%A7%8C%EB%93%A4%EA%B8%B0)
- Array.from 이 아닌 `const timeNodes = [...document.querySelectorAll('[data-time]')];` 를 사용해도 배열로 만들어줄 수 있다. (spread)
- timeNodes 를 콘솔에 찍어봤다.
  <img width="483" alt="image" src="https://user-images.githubusercontent.com/82592845/161933373-31ed1a3d-70c9-454e-9b31-53352d183a43.png">

```jsx
const seconds = timeNodes
  .map((node) => node.dataset.time)
  .map((timeCode) => {
    const [mins, secs] = timeCode.split(":").map(parseFloat);
    return mins * 60 + secs; // second 로 나타내기
  })
  .reduce((total, videoseconds) => total + videoseconds);
```

- timeNodes의 data-time을 `node.dataset.time`로 표시하여 mapping한다.
  <img width="450" alt="image" src="https://user-images.githubusercontent.com/82592845/161934990-4da34b99-dace-4980-bc23-ed2f1e2ea949.png">
- 시간 사이의 `:` 를 `split(":")로 없애고 parseFloat로 String을 실수로 만들고 mapping
  <img width="450" alt="image" src="https://user-images.githubusercontent.com/82592845/161937651-fc10235b-34a1-474c-8e7b-3211deefd0c5.png">
  <img width="465" alt="image" src="https://user-images.githubusercontent.com/82592845/161938125-04817979-d8de-4fe3-afe5-728b7762d75e.png">
- `.reduce((total, videoseconds) => total + videoseconds);`로 값을 다 더한다.
  <img width="489" alt="image" src="https://user-images.githubusercontent.com/82592845/161938695-acb4aa29-7e2c-4fe5-9376-8f730f90b092.png">
  - reduce [mdn바로가기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

```jsx
let secondsLeft = seconds;
const hours = Math.floor(secondsLeft / 3600);
secondsLeft = secondsLeft % 3600;

const mins = Math.floor(secondsLeft / 60);
secondsLeft = secondsLeft % 60;

console.log(hours, mins, secondsLeft);
```

- 1시간인 3600초로 시간을 계산하고 `Math.floor` 사용해서 소수점 없앤다.
- secondsLeft에 값을 다시 할당해서 시간, 분, 초를 나타낸다.
