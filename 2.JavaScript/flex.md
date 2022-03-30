## Flex

---

![플랙스](https://user-images.githubusercontent.com/82592845/160863226-7addc7f1-fb2c-442e-a0cd-9d557f33f86c.gif)

### HTML

1. 큰 틀을 panels 로 묶는다
2. 5가지 틀을 panel 1 2 3 4 5 로 나눈다.
3. p태그를 사용하여 인라인태그로 나눈다.

```html
<div class="panels">
  <div class="panel panel1">
    <p>Hey</p>
    <p>Let's</p>
    <p>Dance</p>
  </div>
  <div class="panel panel2">
    <p>Give</p>
    <p>Take</p>
    <p>Receive</p>
  </div>
  <div class="panel panel3">
    <p>Experience</p>
    <p>It</p>
    <p>Today</p>
  </div>
  <div class="panel panel4">
    <p>Give</p>
    <p>All</p>
    <p>You can</p>
  </div>
  <div class="panel panel5">
    <p>Life</p>
    <p>In</p>
    <p>Motion</p>
  </div>
</div>
```

---

### CSS 내부 설명

```html
<style>
  html {
    box-sizing: border-box;
    background: #ffc600;
    font-family: "helvetica neue";
    font-size: 20px;
    font-weight: 200;
  }

  body {
    margin: 0;
  }

  // 전체 , 전체 전, 전체 후
  *,
  *:before,
  *:after {
    box-sizing: inherit;
  }

  // 전체 틀의 높이를 100vh, 넘치는거 hidden, display는 flex로 세로로 나뉘게 한다.
  .panels {
    min-height: 100vh;
    overflow: hidden;
    display: flex;
  }

  .panel {
    background: #6b0f9c;
    box-shadow: inset 0 0 0 5px rgba(255, 255, 255, 0.1);
    color: white;
    text-align: center;
    align-items: center;
    /* Safari transitionend event.propertyName === flex */
    /* Chrome + FF transitionend event.propertyName === flex-grow */
    transition: font-size 0.7s cubic-bezier(0.61, -0.19, 0.7, -0.11), flex 0.7s
        cubic-bezier(0.61, -0.19, 0.7, -0.11), background 0.2s;
    font-size: 20px;
    background-size: cover;
    background-position: center;

    flex: 1; /* 하나씩 차지 */
    justify-content: center;
    align-items: center;
    display: flex;
    flex-direction: column;
  }

  //  배경 이미지
  .panel1 {
    background-image: url(https://source.unsplash.com/gYl-UtwNg_I/1500x1500);
  }
  .panel2 {
    background-image: url(https://source.unsplash.com/rFKUFzjPYiQ/1500x1500);
  }
  .panel3 {
    background-image: url(https://images.unsplash.com/photo-1465188162913-8fb5709d6d57?ixlib=rb-0.3.5&q=80&fm=jpg&crop=faces&cs=tinysrgb&w=1500&h=1500&fit=crop&s=967e8a713a4e395260793fc8c802901d);
  }
  .panel4 {
    background-image: url(https://source.unsplash.com/ITjiVXcwVng/1500x1500);
  }
  .panel5 {
    background-image: url(https://source.unsplash.com/3MNzGlQM7qs/1500x1500);
  }

  /* Flex Children */
  .panel > * {
    margin: 0;
    width: 100%;
    transition: transform 0.5s;

    /*border: 1px solid red;*/

    flex: 1 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  //각 panel의 상하단 <p>를 translateY를 이용하여 위 아래로 화면에서 없앤 후
  //open-active 부분을 자바스크립트로 click 되면 화면에 보이게 한다.
  .panel > *:first-child {
    transform: translateY(-100%);
  }
  .panel.open-active > *:first-child {
    transform: translateY(0);
  }
  .panel > *:last-child {
    transform: translateY(100%);
  }
  .panel.open-active > *:last-child {
    transform: translateY(0);
  }

  // panel들의 p 를 대문자로 바꿔주고 그림자, 글씨크기 조정
  .panel p {
    text-transform: uppercase;
    font-family: "Amatic SC", cursive;
    text-shadow: 0 0 4px rgba(0, 0, 0, 0.72), 0 0 14px rgba(0, 0, 0, 0.45);
    font-size: 2em;
  }

  // panel의 2번째 자식요소(가운데 p)의 글씨크기 크게 조정
  .panel p:nth-child(2) {
    font-size: 4em;
  }

  .panel.open {
    font-size: 40px;

    flex: 5; /* 열리면 5 만큼 차지한다 */
  }
</style>
```

- box-sizing
  - content-box : 콘텐트 영역을 기준으로 크기를 정합니다.
  - border-box : 테두리를 기준으로 크기를 정합니다.
  - initial : 기본값으로 설정합니다.
  - inherit : 부모 요소의 속성값을 상속받습니다.

---

### JAVASCRIPT

```html
<script>
  const panels = document.querySelectorAll(".panel");

  function toggleOpen() {
    this.classList.toggle("open");
  }

  function toggleActive(e) {
    console.log(e.propertyName);
    //if(e.propertyName === 'flex-grow')    //사파리에서는 flex-grow => flex로
    //편하게
    if (e.propertyName.includes("flex")) {
      this.classList.toggle("open-active");
    }
  }

  panels.forEach((panel) => panel.addEventListener("click", toggleOpen));
  panels.forEach((panel) =>
    panel.addEventListener("transitionend", toggleActive)
  );
</script>
```

- panels 에 클래스가 panel인 요소를 다 담는다.
- panel에 classList.toggle!
  - toggle, 전등이 켜졌다 꺼지는것처럼, 0과 1이 반복되는 것을 의미한다.
    즉, 기본이 0이었다면 1으로 1이었다면 0으로 바뀌는 것으로 toggle 메서드는 클래스가 있으면 제거하고 없다면 추가해주는 역할을 한다.
- 클릭 이벤트리스터를 달아서 작동하게 한다.
