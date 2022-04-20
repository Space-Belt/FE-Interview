# 두더지 잡기 게임

> <h2>완성</h2>

![두더지잡기](https://user-images.githubusercontent.com/82592845/164155214-821e0971-07d0-4e3e-9042-f815cdc16adb.gif)

---

<br/>

> <h2>HTML</h2>

```html
<h1>Whack-a-mole! <span class="score">0</span></h1>
<div class="btnBox">
  <button onClick="startGame()">Start!</button>
</div>

<div class="game">
  <div class="hole hole1">
    <div class="mole"></div>
  </div>
  <div class="hole hole2">
    <div class="mole"></div>
  </div>
  <div class="hole hole3">
    <div class="mole"></div>
  </div>
  <div class="hole hole4">
    <div class="mole"></div>
  </div>
  <div class="hole hole5">
    <div class="mole"></div>
  </div>
  <div class="hole hole6">
    <div class="mole"></div>
  </div>
</div>
```

- 게임 시작 버튼, 두더지 나올 곳!

---

> <h2>JAVASCRIPT</h2>

```jsx
const holes = document.querySelectorAll(".hole");
const scoreBoard = document.querySelector(".score");
const moles = document.querySelectorAll(".mole");
const ended = document.querySelectorAll(".endGame");
const startButton = document.querySelector(".startBtn");
```

- 컨트롤할 요소 불러오기

```jsx
function randomTime(min, max) {
  return Math.round(Math.random() * (max - min) + min);
}
```

- 두더지 나올 시간 설정

```jsx
function randomHole(holes) {
  const idx = Math.floor(Math.random() * holes.length);
  const hole = holes[idx];

  if (hole == lastHole) {
    console.log("Ah nooo thats the same one buddy");
    return randomHole(holes);
  }

  lastHole = hole;
  return hole;
}
```

- 두더지 나올 구멍 설정

```jsx
let lastHole;

function peep() {
  const time = randomTime(200, 1000);
  const hole = randomHole(holes);
  hole.classList.add("up");
  setTimeout(() => {
    hole.classList.remove("up");
    if (!timeUp) {
      peep();
      startButton.textContent = "게임중";
    } else {
      startButton.textContent = "시작";
    }
  }, time);
}
```

- 두더지 중복된 구멍에서 연속으로 나오지 않게하기 위한 변수 선언.
- 두더지가 올라올수 있도록 하는 함수

```jsx
let timeUp = false;

const startGame = () => {
  scoreBoard.textContent = 0;
  timeUp = false;
  score = 0;
  peep();
  setTimeout(() => (timeUp = true), 5000);
};
```

- 게임을 멈춰주도록 하는 변수
- 게임을 실행시키고 멈추는 함수
- textContent로 숫자 0 으로 초기화
- setTimeout의 마지막 인자로 어느기간동안 진행할지 받는다.

```jsx
function bonk(e) {
  if (!e.isTrusted) {
    return;
  } else {
    score++;
  }
  this.classList.remove("up");
  this.classList.add("down");
  scoreBoard.textContent = score;
}
```

- 두더지가 클릭되면 카운트되어 표시하는 함수

```jsx
moles.forEach((mole) => mole.addEventListener("click", bonk));
```

- 매 두더지 마다 forEach 이용해서 click 이벤트 발생시 bonk 함수 발생시킨다!
