# 체크박스

![체크박스](https://user-images.githubusercontent.com/82592845/161013756-2920725d-93f9-41ef-8ed1-b3d23bd62e59.gif)

> 과정

### HTML

```
<div class="inbox">
    <div class="item">
      <input type="checkbox">
      <p>This is an inbox layout.</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Check one item</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Hold down your Shift key</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Check a lower item</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Everything in between should also be set to checked</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Try do it without any libraries</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Just regular JavaScript</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Good Luck!</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Don't forget to tweet your result!</p>
    </div>
  </div>
```

---

### CSS

```
<style>

    html {
      font-family: sans-serif;
      background: #ffc600;
    }

    .inbox {
      max-width: 400px;
      margin: 50px auto;
      background: white;
      border-radius: 5px;
      box-shadow: 10px 10px 0 rgba(0,0,0,0.1);
    }

    .item {
      display: flex;
      align-items: center;
      border-bottom: 1px solid #F1F1F1;
    }

    .item:last-child {
      border-bottom: 0;
    }

    input:checked + p {
      background: #F9F9F9;
      text-decoration: line-through;
    }

    input[type="checkbox"] {
      margin: 20px;
    }

    p {
      margin: 0;
      padding: 20px;
      transition: background 0.2s;
      flex: 1;
      font-family: 'helvetica neue';
      font-size: 20px;
      font-weight: 200;
      border-left: 1px solid #D1E2FF;
    }
  </style>
```

---

### JavaScript

## 1. checkboxes에 input을 담는다.

```html
const checkboxes = document.querySelectorAll('.inbox input[type="checkbox"]');
```

## 2. checkboxes에 click 이벤트 발생하면 handleCheck함수실행

```html
checkboxes.forEach(checkbox => checkbox.addEventListener('click', handleCheck));
```

## 3. 최근 클릭된 체크박스를 위한 변수선언

```html
let first;
```

## 4. shift키 누르고 누르는 기능함수

```html
function handleCheck(e) { console.log(first); let inBetween = false; if
(e.shiftKey && this.checked) { //시프트키 && 체크 checkboxes.forEach(checkbox =>
{ if (checkbox === this || checkbox === first) { inBetween = !inBetween;
console.log('Starting to check them in between!'); } if (inBetween) { // true가
되면 실행 되서 체크 한다 checkbox.checked = true; } }); } first = this; }
```

1. inBetween 을 false로 설정
2. if조건문을 충족시키면 inBetween = !inBetween 사용해서 true 로 만든다.
3. 그러면 밑 If문이 실행되고 checkbox.checked = true;로 인해 체크가 된다.
