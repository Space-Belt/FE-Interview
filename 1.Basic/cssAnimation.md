# **CSS 애니메이션(@keyframes), Canvas API**

---

### Canvas

---

`<canvas>` 태그는 JavaScript를 사용하여 다양한 그래픽 요소를 만들 수 있는 태그입니다. 단순 도형, 애니메이션, 게임, 데이터 시각화 등 사용에 따라 수많은 콘텐츠를 만들 수 있습니다.

---

### 사용 전 설정

---

> 캔버스 생성

```jsx
<canvas id="canvas">캔버스를 지원하지 않는 브라우저에서 보이는 메세지!</canvas>
```

- 캔버스는 DOM으로 요소들을 조작하는 방식으로 작성된다.
- 즉 요소를 선택할 때 사용할 `id` 를 작성해주는 것이 좋다.

---

> 엘리먼트 선택 (자바스크립트 사용)

```jsx
const canvas = document.querySelector("#canvas");
```

---

> 캔버스를 제대로 다루기 전에 기본적으로 너비, 높이를 설정해준다!
> 미설정시 기본적으로 300\*150 픽셀의 사이즈로 생성된다.
> 두가지 설정방법에 대해 알아보자!

1. 태그 속성으올 설정하는 방법 ( 이 방법은 어떤 단위로 설정하던 픽셀로만 인식한다 )

```jsx
<canvas id="canvas" width="1000" height="800"></canvas>
		// 1000픽셀 * 800픽셀로 설정된다

<canvas id="canvas" width="80vw" height="80vh"></canvas>
		// vw, vh를 전달했지만 80픽셀 * 80픽셀로 설정된다.
```

1. DOM으로 설정하는 방법 ( 이 방법은 유동적인 값도 설정 가능하여 고정되어 있지 않은 캔버스크기인 경우에 사용하기 좋다 )

```jsx
canvas.width = 80vw;
canvas.height = 40vh;
	// 80vw * 40vh 로 설정된다

canvas.width = innerWidth;
canvas.height = innerHeight;
```

---

> 캔버스에 그래픽 작업을 가능케 하는 [속성들과 메소드들](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D)이 들어있는 객체를 불러오자!

<aside>
💡 const ctx = canvas.getContext(”2d”)

</aside>

---

### 이제 간단한 예제로 익혀보자!

---

> 사각형 그리기

![스크린샷 2022-02-24 오전 10.37.13.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12c73377-4a5c-4693-8e73-c5a8c0b75013/스크린샷_2022-02-24_오전_10.37.13.png)

1. 색칠된 사각형

- `fillStyle` 속성은 내부의 색상을 설정할 수 있다.
  - `ctx.fillStyle = ‘pink’`
- `fillRect` 메소드는 x좌표, y좌표, 가로길이, 세로길이인자를 순서대로 입력
  - `ctx.fillRect = (10, 10, 100, 50)`

![스크린샷 2022-02-24 오전 10.39.50.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1fd1cef1-02b9-441c-9c9f-6eb886ffae6c/스크린샷_2022-02-24_오전_10.39.50.png)

1. 선으로 그린 사각형

- `lineWidth` 속성은 선의 굵기를, `strokeStyle` 속성은 선의 생상을 설정
  ```
  ctx.lineWidth = 5;
  ctx.strokeStyle = "black";
  ```
- `strokeRect` 메소드 이용하여 사각형 그리기 (x좌표, y좌표, 가로길이, 세로길이인자를 순서대로 입력)
  - `ctx.strokeRect(10, 10, 100, 50)`

![스크린샷 2022-02-24 오전 10.41.51.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97e2b387-07ac-46c9-8988-43adebac2798/스크린샷_2022-02-24_오전_10.41.51.png)

1. 사각형으로 지우기

- `clearRect` 메소드는 지울 범위를 설정해준다.(x좌표, y좌표, 가로길이, 세로길이인자를 순서대로 입력)
  - `ctx.clearRect( 20, 20, 80, 30)`

→ [MDN 캔버스를 이용한 도형 그리기 참고](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes)

---

### Canvas로 클릭이벤트 만들기

---

> 클릭이벤트가 일어날 때의 마우스 위치 구하기

- 클릭 시 캔버스 위에서의 마우스 위치를 구하려면, 화면에서의 마우스 위치에서 화면에서의 캔버스 위치를 빼는 방법이 있다.
- 화면상 마우스 위치를 구하는 이벤트 객체의 속성
  - X좌표 : `event.clientX`
  - Y좌표 : `event.clientY`
- 화면상 캔버스의 위치를 구하는 속성
  - X좌표 : `ctx.canvas.offsetLeft`
  - Y좌표 : `ctx.canvas.offsetTop`
- 혹은 `event.offsetX`, `event.offsetY`로 바로 구할 수 있다.

---

> 클릭시 사각형 생성 코드

```jsx
canvas.onclick = function (event) {
  // 좌표 할당
  const x = event.clientX - ctx.canvas.offsetLeft;
  const y = event.clientY - ctx.canvas.offsetTop;

  // 30픽셀*30픽셀 크기의 사각형 그리기
  // x, y를 그대로 전달시 클릭된 곳을 시작으로 사각형이 생겨 어색하다.
  // 클릭위치를 사각형의 정중앙이 되도록 사각형크기/2 한 만큼 좌표에서 빼준다.
  // 따라서 x - 15, y - 15 전달합니다.
  ctx.fillRect(x - 15, y - 15, 30, 30);
};
```

![clickSquare.gif](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7050b2fe-7db1-4865-9ea9-984adee5c85f/clickSquare.gif)

---

### CSS 애니메이션

---

여러개의 CSS 스타일을 부드럽게 전환시킨다. `@keyframes` 키워드 활용시 시간 순서대로 정밀하게 짜여진 애니메이션을 생성할 수 있다.

---

> `@keyframes` 사용법

% 단위로 상태 작성

```
@keyframes animationName {
	0% {                        from {    변경 가능
		CSS property : value;
	}

	50% {
		CSS property : value;
	}

	100% {                      to {      변경 가능
		CSS property : value;
	}
}
```

---

> animation 속성

- `animation` : 띄어쓰기로 쭉 나열하면 아래의 속성들을 한 번에 지정할 수 있음
  - `animation-iteration-count` : 애니메이션이 몇 번 반복될지 지정
  - `animation-play-state` : 애니메이션을 재생 상태. 멈추거나 다시 재생 시킬 수 있음
  - `animation-timing-function` : 중간 상태들의 전환을 어떤 시간간격으로 진행할지 지정
  - `animation-fill-mode` : 애니메이션이 재생 전 후의 상태 지정

**animation-name →** 애니메이션의 중간 상태를 지정하는 이름. `@keyframes` 블록에 작성

애니메이션을 적용하고 싶은 요소에 `animation` 속성의 첫번째 값으로, 혹은 `animation-name` 이라는 속성으로 `@keyframes` 키워드를 사용해서 만든 애니메이션 이름을 작성해주면 됩니다.

```
#logo {
	animation : lotate;
}
```

```
#logo {
	animation-name : lotate;
}
```

하지만 이 속성만으로는 애니메이션이 재생되지 않습니다. 여러 속성들 중 최소한 `animation-name` 과 `animation-duration` 은 지정해줘야 애니메이션이 실행되기 때문인데요. 그럼 다음 속성으로 넘어가봅시다.

**animation-duration** → 한 싸이클의 애니메이션이 재생될 시간 지정

애니메이션이 재생될 시간을 `animation` 속성의 두번째 값으로, 혹은 `animation-dutation` 이라는 속성으로 시간 단위로 작성해줍니다. 작성해주지 않을 경우 기본값이 `0` 이기 때문에 애니메이션이 재생되지 않습니다.

```
#logo {
	animation : lotate 3s ;
}
```

```
#logo {
	animation-name : lotate;
	animation-duration : 3s;
}
```

`animation-dutation` 의 값을 각각 `3s`, `1s`로 지정해줬을 때, `3s` 는 3초 동안, `1s` 는 1초동안 애니메이션이 재생

\***\*animation-delay\*\*** → 애니메이션이 시작을 지연시킬 시간 지정

애니메이션 재생을 미룰 시간을 지정합니다. 역시 `animation` 속성에 띄어쓰기로 구분해준 다음, 혹은 `animation-dutation` 이라는 속성으로 시간 단위로 작성해줍니다.

```
#logo {
	animation : lotate 3s 3s ;
}
```

```
#logo {
	animation-name : lotate;
	animation-duration : 3s;
	animation-delay : 3s;
}
```

\***\*animation-direction\*\*** → 애니메이션 재생 방향을 지정

애니메이션 재생 방향을 지정합니다. 역시 `animation` 속성에 띄어쓰기로 구분해준 다음, 혹은 `animation-direction` 이라는 속성으로 작성해줍니다.

```
#logo {
	animation : lotate 3s reverse ;
}
```

```
#logo {
	animation-name : lotate;
	animation-duration : 3s;
	animation-direction : arternate;
}
```

전달해줄 수 있는 값은 다음과 같습니다.

- `normal` : 기본 값. 재생이 끝나면 처음부터 다시 재생합니다.
- `reverse` : 역방향으로 재생합니다.
- `alternate` : 순방향부터 역방향을 번갈아가며 재생합니다.
- `alternate-reverse` : 역방향부터 순방향을 번갈아가며 재생합니다.

\***\*animation-iteration-count\*\*** → 애니메이션이 몇 번 반복될지 지정

애니메이션이 몇 번 재생될지 지정합니다. 기본 값은 `1`이며, 설정한 횟수만큼 애니메이션이 반복 재생 됩니다. `infinite` 로 설정할 경우 무한 반복 되며, 소수점을 작성할 경우 재생 도중 처음 상태로 돌아갑니다. 예를 들어, 재생 시간이 3초일 때, 0.6을 전달할 경우 3 \* 0.6을 한 1.8초만큼만 재생되고 처음 상태로 돌아가게 됩니다.

```
#logo {
	animation : lotate 3s infinite ;
	/* 애니메이션이 무한 반복 됩니다. */
}
```

```
#logo {
	animation-name : lotate;
	animation-duration : 3s;
	animation-iteration-count : 3 ;
	/* 애니메이션이 3번 반복 됩니다. */
}
```

\***\*animation-play-state\*\*** → 애니메이션을 재생 상태. 멈추거나 다시 재생 시킬 수 있음

애니메이션이 재생 상태를 설정합니다. 기본 값인`running`, 애니메이션을 정지시키는 `pause` 를 값으로 지정할 수 있습니다. 보통 이벤트로 애니메이션을 재생 상태를 변경할 때 사용합니다.

```
#logo {
	animation : lotate 3s pause ;
}
```

```
#logo {
	animation-name : lotate;
	animation-duration : 3s;
	animation-play-state : pause ;
}
```

\***\*animation-fill-mode\*\*** → 애니메이션이 재생 전 후의 상태 지정

애니메이션 재생 전 후의 상태를 지정합니다.

- `none` : 기본 값. 재생중이 아닌 경우 **요소의 스타일**을 유지합니다.
- `forwards` : 재생중이 아닌 경우 **마지막 키프레임 스타일**을 유지합니다.
- `backwards` : 재생중이 아닌 경우 **첫 번째 키프레임 스타일**을 유지합니다.
- `both` : 재생 전에는 **첫 번째 키프레임 스타일**을, 재생 후에는 **마지막 키프레임 스타일**을 유지합니다.

\***\*animation-timing-funciton\*\*** → 중간 상태들의 전환을 어떤 시간간격으로 진행할지 지정

```
#logo {
	animation-name : lotate;    /* lotate 라는 이름의 키프레임 애니메이션을 */
	animation-duration : 3s;               /* 3초 동안 재생하며, */
	animation-iteration-count : infinite;  /* 애니메이션을 무한 반복하고, */
	animation-timing-function : linear;    /* 선형으로 재생합니다. */
}

#logo {
	animation : lotate 3s infinite linear;
}
```

@keyframes로 3초동안 0% : 0deg → 80% : 180deg → 100% : 360deg 를 주면 0 ~ 80% → 2.4초 동안 180도 회전, 80~100%→ 0.6초동안 180도 회전

⇒ 키프레임을 설정하면서 주는 **중간값은 애니메이션 재생 시간을 기준으로 함**
