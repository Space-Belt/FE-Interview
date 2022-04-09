# stripe Follow Along Dropdown Nav

> 목표!!

> 마우스 올렸을때(hover) 리스트 요소 드롭메뉴를 구현!!

## 1. cool의 직계자손, 하얀색 배경의 dropdownBackground, 최상위 요소인 nav 를 담는다.

```html
const triggers = document.querySelectorAll('.cool > li'); const background =
document.querySelector('.dropdownBackground'); const nav =
document.querySelector('.top');
```

- .cool > li 로 하지 않고 a 로 하면 불편하다.
- const background는 nodelist가 필요한게 아니고 하나의 요소가 필요한것이므로 querySelector를 사용해야한다.
- **노드 리스트(node list)**
  노드 리스트는 getElementsByTagName() 메소드나 childNodes 프로퍼티의 값으로 반환되는 객체입니다.
  이 객체는 HTML 문서와 같은 순서로 문서 내의 모든 노드를 리스트 형태로 저장하고 있습니다.
  리스트의 각 노드는 0부터 시작하는 인덱스를 이용하여 접근할 수 있습니다.

## 2. hover inout, mous enterleave 를 수신할 함수 2개를 만들고 바인딩.

```html
function handleEnter() {} function handleLeave() {} triggers.forEach(trigger =>
trigger.addEventListener('mouseenter', handleEnter)); triggers.forEach(trigger
=> trigger.addEventListener('mouseleave', handleLeave));
```

## 3.드롭다운의 내용을 표시, !표시 기능을 handleEnter와 handleLeave 넣는다.

```html
function handleEnter() { this.classList.add('trigger-enter'); // 여기까지 하면
CSS의 opacity가 아직 0이라 보이지 않는다. // setTimeout을 이용하여
trigger-enter와 시간간격을 준다. // transition opacity 효과와 나중에 만들
dropdownBackground의 크기를 정해주기 위해서다. setTimeout(() =>
this.classList.add('trigger-enter-active'), 150); } function handleLeave() {
this.classList.remove('trigger-enter', 'trigger-enter-active'); }
```

## 4. CSS 에서 .trigger-enter-active .dropdown 추가

```html
.trigger-enter-active .dropdown { opacity: 1; }
```

- 여기 까지 하면 마우스 올렸을때 li 아이템들이 보인다.
  ![스크린샷 2021-09-24 오후 10.45.12.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/df923172-3287-49b5-b72f-6a3cf0b32c9e/스크린샷_2021-09-24_오후_10.45.12.png)

## 5. background가 나올수 있게 하는 과정의 시작

```html
function handleEnter() { background.classList.add('open'); } function
handleLeave() { background.classList.remove('open'); } ***************** CSS
***************** .dropdownBackground.open { opacity: 1; }
```

- 여기까지 하면 화면 상단 왼쪽에 하얀색 div 박스가 보인다.

## 6. div 박스의 크기를 li들에 맞춰주는 과정

```html
const triggers = document.querySelectorAll('.cool > li'); const background =
document.querySelector('.dropdownBackground'); const nav =
document.querySelector('.top'); // 굳이 이것을 다시 적은 이유는 아래의 // const
dropdown이 저 함수안에서 선언되어야 handleEnter된 this를 상속 받기 떄문이라는
것을 // 메모하기 위해서다 ㅎ.,ㅎ function handleEnter() { const dropdown =
this.querySelector('.dropdown'); const dropdownCoords =
dropdown.getBoundingClientRect(); const navCoords = nav.getBoundingClientRect();
}
```

- hover된 dropdown을 getBoundingClienRect();를 사용해서 정확한 속성을 알아낸다.

## 7. getBoundingClienRect(); 로 알아낸 속성을 이용한다.

```html
function handleEnter() { const dropdown = this.querySelector('.dropdown'); const
dropdownCoords = dropdown.getBoundingClientRect(); const navCoords =
nav.getBoundingClientRect(); const coords = { height: dropdownCoords.height,
width: dropdownCoords.width } background.style.setProperty('width',
`${coords.width}px`); background.style.setProperty('height',
`${coords.height}px`); }
```

- 여기 까지하면 위치는 상단에 있지만 각각의 너비와 높이는 찾았다.
- background.style.setProperty('width', `${coords.width}px`);
  - 여기서 `${coords.width}px` 이렇게 template String으로 묶은 이유는
  - px 값을 넣는 방법이기 떄문이다. (넣어야한다)

## 8. top, left 값을 넣어서 제자리에 넣어주자.

```html
const coords = { height: dropdownCoords.height, width: dropdownCoords.width,
top: dropdownCoords.top - navCoords.top, left: dropdownCoords.left -
navCoords.left } background.style.setProperty('width', `${coords.width}px`);
background.style.setProperty('height', `${coords.height}px`);
background.style.setProperty('transform', `translate(${coords.left}px,
${coords.top}px)`);
```

- top: dropdownCoords.top - navCoords.top,
  left: dropdownCoords.left - navCoords.left - navCoords.top, left 값을 빼줘야 제일 위의 <h2>같은 다른태그에 의해 변형됐을때 정상적으로 작동한다.
- translate(x, y);
- 여기까지하면 완벽해보이나 마우스를 빨리 움직이면 dropbox가 생기기도전에 list들이 계속 보인다.

## 9. 어지럽게 계속 나오는 것들을 setTimeout 부분을 변경해서 정상적으로 만들어보자!

```html
// setTimeout(() => { // contains 값이 존재하면 (true/false) //
if(this.classList.contains('trigger-enter')) { //
this.classList.add('trigger-enter-active') // } // }, 150); 간단히!
setTimeout(() => this.classList.contains('trigger-enter') &&
this.classList.add('trigger-enter-active'), 150);
```
