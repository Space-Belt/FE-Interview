# 캔버스!

## HTML

```jsx
<canvas id="draw" width="800" height="800"></canvas>
```

- width와 height를 설정해준다.

## JAVASCRIPT

```jsx
// 캔버스 잡아놓기 #draw
const canvas = document.querySelector("#draw");
// 2d 로
const ctx = canvas.getContext("2d");
// 넓이 높이 정하기
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
// 도형의 윤곽선 색을 설정합니다
ctx.strokeStyle = "#BADA55";
// 선이 꺽이는 부분의 스타일 지정 (bevel, round, miter)
ctx.lineJoin = "round";
// 선 끝 부분의 스타일 지정 ( butt, round, square )
ctx.lineCap = "round";
// 선 두께 지정 ( 기본 1.0 )
ctx.lineWidth = 100;
// 포토샵 블랜드모드 색 섞이는것
// multiply 말고 다른 옵션 많으니 찾아보기
// ctx.globalCompositeOperation = 'multiply';

// mousedown이벤트가 발생 하면 그리기가 시작됩니다 .
// 먼저 마우스 포인터의 x 및 y 좌표를 변수 x및 y에 저장 한 다음 isDrawingtrue 로 설정 합니다.
// 클릭이 되면 true가 되고 때면은 false가 되도록 다른곳에서 처리
let isDrawing = false;
// 마우스클릭이 끝난곳 저장
let lastX = 0;
let lastY = 0;
//
let hue = 0;

let direction = true;

function draw(e) {
  if (!isDrawing) return; // stop the fn from running when they are not moused down
  console.log(e);
  // mother effing hsl    hsl(hue, saturation, lightness)
  // 색조는 0에서 360 사이의 색상환 각도입니다. 0은 빨간색, 120은 녹색, 240은 파란색입니다.
  //채도는 백분율 값이고 0 %는 회색 음영을 의미하고 100 %는 풀 컬러입니다.
  //밝기도 백분율이며 0 %는 검은 색, 50 %는 밝거나 어둡지 않으며 100 %는 흰색입니다.
  ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;
  //ctx.lineWidth = hue; 여기서 하지않고 밑에서 처리
  ctx.beginPath();
  // start from
  ctx.moveTo(lastX, lastY);
  // go to
  ctx.lineTo(e.offsetX, e.offsetY);
  ctx.stroke();
  //  lastX = e.offsetX;
  //  lastY = e.offsetY;
  [lastX, lastY] = [e.offsetX, e.offsetY];

  hue++; // 계속 늘어나니까 밑에서 한계를 정해준다
  if (hue >= 360) {
    hue = 0;
  }

  // ctx.lineWidth = hue;
  if (ctx.lineWidth >= 100 || ctx.lineWidth <= 1) {
    direction = !direction;
  }

  if (direction) {
    //if(direction = true)
    ctx.lineWidth++;
  } else {
    ctx.lineWidth--;
  }
}

// 여러개의 일을 시키기 위해서 아거로 변경
canvas.addEventListener("mousedown", (e) => {
  isDrawing = true;
  [lastX, lastY] = [e.offsetX, e.offsetY];
});

canvas.addEventListener("mousemove", draw);
// 위에꺼로 바꿈
//canvas.addEventListener('mousedown', () => isDrawing = false);
canvas.addEventListener("mouseup", () => (isDrawing = false));
canvas.addEventListener("mouseout", () => (isDrawing = false));
```
