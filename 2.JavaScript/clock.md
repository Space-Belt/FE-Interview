## 🕰 Clock

---

![시계](https://user-images.githubusercontent.com/82592845/160370103-0e289fa6-493c-4f22-82de-4a6df82c50c4.gif)

---

```jsx
// hand hour-hand 시침
// hand second-hand 초침
// hand min-hand 분침

<div class="clock">
  <div class="clock-face">
    <div class="hand hour-hand" id="hour"></div>
    <div class="hand second-hand"></div>
    <div class="hand min-hand"></div>
  </div>
</div>
```

```jsx
// CSS

.hand {
      width: 50%;
      height: 6px;
      background: black;
      position: absolute;
      top: 50%;
      transform-origin: 100%;
      transform: rotate(90deg);
      transition: all 0.05s;
      transition-timing-function: cubic-bezier(0.18, 2.58, 0, 2.21);

```

- transform-origin : 요소의 변환에 대한 원점
  - rotate()함수 의 변환 원점 은 회전 중심입니다.
  - [https://developer.mozilla.org/en-US/docs/Web/CSS/transform-origin](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-origin)
- transform: rotate(90deg); 실험 하려고 만듬.
- [https://www.codingfactory.net/10953](https://www.codingfactory.net/10953)

> 초침, 분침, 시침을 각 변수에 넣어준다.
> (secondHand, minsHand, hourHand)

```
const secondHand = document.querySelector(".second-hand");

const minsHand = document.querySelector(".min-hand");

const hourHand = document.querySelector(".hour-hand");
```

> 현재시간을 받아온다.( 시간, 분, 초)

```
const now = new Date();
const seconds = now.getSeconds();
const mins = now.getMinutes();
const hours = now.getHours();
```

> 시분초침이 움직일 함수를 만든다.
> secondHand변수에 style 의 transform을 추가한다.

```
function setDate() {
  const now = new Date();

  const seconds = now.getSeconds();
  const secondsDegrees = (seconds / 60) * 360 + 90;
  secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

  const mins = now.getMinutes();
  const minsDegrees = (mins / 60) * 360 + 90;
  minsHand.style.transform = `rotate(${minsDegrees}deg)`;

  const hours = now.getHours();
  const hourDegrees = (hours / 12) * 360 + 90;
  hourHand.style.transform = `rotate(${hourDegrees}deg)`;
}
```

[https://curryyou.tistory.com/185](https://curryyou.tistory.com/185) 백틱 (``` ) 사용법

백틱으로 표현식을 삽입하여 정해준다.

```
setInterval(setDate, 1000);
```

1. setInterval 사용 해서 실행

- [https://offbyone.tistory.com/241](https://offbyone.tistory.com/241)
- 자바스크립트로 주기적인 작업을 실행하기 위해서 setInterval과 setTimeout 메소드를 사용할 수 있습니다. 두 가지는 비숫하지만 중요한 차이점을 가집니다.
- **setInterval 함수** : 일정한 시간 간격으로 작업을 수행하기 위해서 사용합니다.clearInterval 함수를 사용하여 중지할 수 있습니다. 주의할 점은 일정한 시간 간격으로 실행되는 작업이 그 시간 간격보다 오래걸릴 경우 문제가 발생할 수 있습니다.
- **setTimeout 함수** : 일정한 시간 후에 작업을 한번 실행합니다. 보통 재귀적 호출을 사용하여 작업을 반복합니다. 기본적으로 setInterval 과는 달리 지정된 시간을 기다린후 작업을 수행하고, 다시 일정한 시간을 기다린후 작업을 수행하는 방식입니다. 지정된 시간 사이에 작업 시간이 추가 되는 것입니다. clearTimeout() 을 사용해서 작업을 중지합니다.
- **clearInterval(), clearTimeout()**이 실행중인 작업을 중지시키는 것은 아닙니다. 지정된 작업은 모두 실행되고 다음 작업 스케쥴이 중지 되는 것입니다.
