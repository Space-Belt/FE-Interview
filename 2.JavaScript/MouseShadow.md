# 마우스위치 그림자

---

> 목표

![마우스그림자](https://user-images.githubusercontent.com/82592845/161511050-e51dd31c-14e4-4ac2-8476-087acecc3325.gif)

- 마우스 위치에 따른 그림자를 움직여보자
- CSS text-shadow 속성
- 좌표 변경 확인하기

## HTML

```
<div class="hero">
  <h1 contenteditable>🔥WOAH!</h1>
</div>
```

-
- ***

## CSS

```
<style>
  html {
    color: black;
    font-family: sans-serif;
  }

  body {
    margin: 0;
  }

  .hero {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    color: black;
  }

  h1 {
    text-shadow: 10px 10px 0 rgba(0,0,0,1);
    font-size: 100px;
  }
</style>
```

- text-shadow : offset-x offset-y blur-radius color;
- 텍스트 그림자 : x좌표 y좌표 블러둥글기 색 순서다.
- mdn에 따르면 아래와 같은 사용법이 더있다.

  ```
  /* offset-x | offset-y | color */
  text-shadow: 5px 5px #558abb;

  /* color | offset-x | offset-y */
  text-shadow: white 2px 5px;

  /* offset-x | offset-y
  /* Use defaults for color and blur-radius */
  text-shadow: 5px 10px;

  /* Global values */
  text-shadow: inherit;
  text-shadow: initial;
  text-shadow: unset;
  ```

## JAVASCRIPT

```
const hero = document.querySelector('.hero');
const text = hero.querySelector('h1');
const walk = 100;
```

- querySelector 이용하여 요소를 선택한다.
- walk 는 그림자가 움직일 기준점이다.

<br />

```
hero.addEventListener('mousemove', shadow);
```

- 마우스 움직임에 따른 이벤트발생할때 shadow 함수를 실행시키게 한다

```
function shadow(e) {
  const { offsetWidth: width, offserHeight: height } = hero ;
  let { offsetX: x, offsetY: y } = e;

  if(this !== e.target) {
    x = x + e.target.offsetLeft;
    y = y + e.target.offsetTop;
  }

  const xWalk = Math.round((x / width * walk) - (walk / 2));
  const yWalk = Math.round((y / width * walk) - (walk / 2));

  text.style.textShadow = `
  ${xWalk}px ${yWalk}px 0 rgba(255,0,255,0.7),
  ${xWalk * -1}px ${yWalk}px 0 rgba(0,255,255,0.7),
  ${yWalk}px ${xWalk * -1}px 0 rgba(0,255,0,0.7),
  ${yWalk * -1}px ${xWalk}px 0 rgba(0,0,255,0.7)
  `;
}
```

- offsetWidth 값과 offsetHeight 값을 사진으로 알아보자
  <img width="595" alt="image" src="https://user-images.githubusercontent.com/82592845/161515285-bd7dced7-4836-458d-8974-221c18719148.png">
- 레이아웃의 width 값과 height 값의 integer 값이다.
- `const width = hero.offsetWidth; const height = hero.offsetHeight;` 를 const { offsetWidth: width, offserHeight: height } = hero; 이렇게 간단히 받을 수 있다.
- `let { offsetX: x, offsetY: y } = e;` 위와 같은 방법으로 e 의 property인 offsetX 와 offsetY의 값을 x, y 에 삽입한다.
- offsetX와 offsetY는 브라우저 인터페이스 내에서의 마우스 위치를 나타내며, 화면 크기에 따라 값이 늘어나거나 줄어들수 있다.
  ```
  if(this !== e.target) {
    x = x + e.target.offsetLeft;
    y = y + e.target.offsetTop;
  }
  ```
- this는 이벤트가 일어나는 상위인 `<div> .hero`태그다.
- `<div> .hero` 태그의 자식요소인 `<h1>`태그에서 이벤트가 발생하면 자식요소에 대한 offset값이 출력된다.
- 그때는 `<h1>`태그의 화면상 왼쪽과 상단 좌표인 offsetLeft와 offsetTop를 x, y에 추가해준다.
- xWalk와 yWalk는 마우스 위치를 중간기준 왼쪽상단(0, 0), 오른쪽상단(1, 0), 오른쪽하단(1, 1), 왼쪽하단(0, 1) 기준인것을 50 또는 -50 으로 만든것이다. (오차있음)
- xWalk와 yWalk 값을 이용해서 그림자 위치를 조정한다.
