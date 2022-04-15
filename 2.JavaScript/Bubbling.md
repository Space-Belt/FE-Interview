> 목표!

> 이벤트 캡쳐, 위임, 버블링, 단일트리거 (once)

> dom이벤트 메커니즘 , addEventListener()에 대한 이해

## 1. 모든 div를 가져온다.

```jsx
const divs = document.querySelectorAll("div");
```

## 2. div에 대한 리스너를 개별적으로 설정한다.

```jsx
divs.forEach((div) =>
  div.addEventListener("click", logText, { capture: true, once: true })
);
function logText(e) {
  console.log(this.className);
}
```

- capture : true ⇒ 자식요소를 누르면 one two three 로 최상위 부터 클릭된 자식 요소 순서
- capture : false ⇒ 자식요소 누르면 three two one 로 클릭된 자식요소부터 부모요소 순서

- once: true ⇒ 클릭이벤트를 한번 받을수 있게 한다.
- once: false ⇒ 제한없이 받을 수 있다.

## 3. 이 현상을 버블링 현상이라 한다 ! 버블링이란???

```jsx
<div class="one">
  <div class="two">
    <div class="three"></div>
  </div>
</div>
```

- 자식요소인 three 에 클릭이벤트가 발생하면 부모요소인 two , one 에도 click이벤트가 발생하는것
- 자식에서부터 부모로

## 4. 이 버블링 현상을 멈출수 있다 ! 그것은 바로 e.stopPropagation(); 다!

```jsx
function logText(e) { console.log(this.classList.value); e.stopPropagation(); //
타고올라가는 bubbling 멈춰! }
```
