# Custom Video Player

![비디오플레이어](https://user-images.githubusercontent.com/82592845/162146595-1794b275-05fe-4a84-92e4-852fa3bf041a.gif)

> <h3>기능</h3>

1. 화면 클릭하면 재생, 정지
2. 앞으로 감기, 뒤로 감기
3. 재생바 클릭 위치에 따라 현재재생시간 변경
4. 볼륨

> <h3>HTML</h3>

```html
<div class="player">
  <video class="player__video viewer" src="652333414.mp4"></video>
  <div class="player__controls">
    <div class="progress">
      <div class="progress__filled"></div>
    </div>
    <button class="player__button toggle" title="Toggle Play">►</button>
    <input
      type="range"
      name="volume"
      class="player__slider"
      min="0"
      max="1"
      step="0.05"
      value="1"
    />
    <input
      type="range"
      name="playbackRate"
      class="player__slider"
      min="0.5"
      max="2"
      step="0.1"
      value="1"
    />
    <button data-skip="-10" class="player__button">« 10s</button>
    <button data-skip="25" class="player__button">25s »</button>
  </div>
</div>
```

1. `<video>` 비디오 태그 src 비디오 연결
2. `<input>` 인풋 range 타입 min, max 최소 최대값 설정 / step 얼만큼 값이 변할지 설정 / value로 기본값 설정
3. `<button>` data-skip 으로 얼만큼 설정할지 지정 해놓는다.(ArrayReduce 편에서 다룬 attribute) [참고](http://html5doctor.com/html5-custom-data-attributes/)

> <h3>CSS</h3>

```css
html {
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body {
  margin: 0;
  padding: 0;
  display: flex;
  background: #7a419b;
  min-height: 100vh;
  background: linear-gradient(135deg, #7c1599 0%, #921099 48%, #7e4ae8 100%);
  background-size: cover;
  align-items: center;
  justify-content: center;
}

.player {
  max-width: 750px;
  border: 5px solid rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  position: relative;
  font-size: 0;
  overflow: hidden;
}

/* 전체화면 */
.player:fullscreen {
  max-width: none;
  width: 100%;
}

.player:-webkit-full-screen {
  max-width: none;
  width: 100%;
}

.player__video {
  width: 100%;
}

.player__button {
  background: none;
  border: 0;
  line-height: 1;
  color: white;
  text-align: center;
  outline: 0;
  padding: 0;
  cursor: pointer;
  max-width: 50px;
}

.player__button:focus {
  border-color: #ffc600;
}

.player__slider {
  width: 10px;
  height: 30px;
}

.player__controls {
  display: flex;
  position: absolute;
  bottom: 0;
  width: 100%;
  transform: translateY(100%) translateY(-5px);
  transition: all 0.3s;
  flex-wrap: wrap;
  background: rgba(0, 0, 0, 0.1);
}

.player:hover .player__controls {
  transform: translateY(0);
}

.player:hover .progress {
  height: 15px;
}

.player__controls > * {
  flex: 1;
}

.progress {
  flex: 10;
  position: relative;
  display: flex;
  flex-basis: 100%;
  height: 5px;
  transition: height 0.3s;
  background: rgba(0, 0, 0, 0.5);
  cursor: ew-resize;
}

.progress__filled {
  width: 50%;
  background: #ffc600;
  flex: 0;
  flex-basis: 50%;
}

/* unholy css to style input type="range" */

input[type="range"] {
  -webkit-appearance: none;
  background: transparent;
  width: 100%;
  margin: 0 5px;
}

input[type="range"]:focus {
  outline: none;
}

input[type="range"]::-webkit-slider-runnable-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  box-shadow: 1px 1px 1px rgba(0, 0, 0, 0), 0 0 1px rgba(13, 13, 13, 0);
  background: rgba(255, 255, 255, 0.8);
  border-radius: 1.3px;
  border: 0.2px solid rgba(1, 1, 1, 0);
}

input[type="range"]::-webkit-slider-thumb {
  height: 15px;
  width: 15px;
  border-radius: 50px;
  background: #ffc600;
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -3.5px;
  box-shadow: 0 0 2px rgba(0, 0, 0, 0.2);
}

input[type="range"]:focus::-webkit-slider-runnable-track {
  background: #bada55;
}

input[type="range"]::-moz-range-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  box-shadow: 1px 1px 1px rgba(0, 0, 0, 0), 0 0 1px rgba(13, 13, 13, 0);
  background: #ffffff;
  border-radius: 1.3px;
  border: 0.2px solid rgba(1, 1, 1, 0);
}

input[type="range"]::-moz-range-thumb {
  box-shadow: 0 0 0 rgba(0, 0, 0, 0), 0 0 0 rgba(13, 13, 13, 0);
  height: 15px;
  width: 15px;
  border-radius: 50px;
  background: #ffc600;
  cursor: pointer;
}
```

1. `background: linear-gradient` 배경 그라데이션 [참고](https://developer.mozilla.org/en-US/docs/Web/CSS/gradient/linear-gradient)
2. `input[type="range"]` type이 range인 요소를 선택하여 style을 준다. / css 선택자 [더 알아보기](https://www.nextree.co.kr/p8468/)

> <h3>JavaScript</h3>

```javaScript
const player = document.querySelector(".player");
const video = player.querySelector(".viewer");
const progress = player.querySelector(".progress");
const progressBar = player.querySelector(".progress__filled");
const toggle = player.querySelector(".toggle");
const skipButtons = player.querySelectorAll("[data-skip]");
const ranges = player.querySelectorAll(".player__slider");
```

1. 선택

```JavaScript
function togglePlay() {
  const method = video.paused ? "play" : "pause";
  video[method]();
}

function updateButton() {
  const icon = this.paused ? "►" : "❚ ❚";
  toggle.textContent = icon;
}
```

1. 재생, 정지를 paused의 true, false를 이용하여 재생, 정지를 작동하게 한다.<br/>
   [비디오 reference 참고](https://www.w3schools.com/tags/ref_av_dom.asp)
2. 재생, 정지 아이콘을 paused에 따라 textContent로 바꿔준다.

```JavaScript
function skip() {
  //console.log(this.dataset.skip);
  video.currentTime += parseFloat(this.dataset.skip);
}
```

- HTML `button` 태그에 `data-skip = (Number)` 준 값을 이용하여 현재 재생시간인 currentTime에 더해준다.

```JavaScript
function handleRangeUpdate() {
  video[this.name] = this.value;
}
```

- range로 조정하는 소리, 재생속도를 this를 사용하여 어디서 일어난 변화인지 알아서 처리하게 해준다.

```JavaScript
function handleProgress() {
  const percent = (video.currentTime / video.duration) * 100;
  progressBar.style.flexBasis = `${percent}%`;
}
```

1. 현재 재생시간을 퍼센트로 만들어 flexBasis를 사용해 현재재생시간을 재생바에 나타낸다.
2. flexBasis는 플랙스아이템의 초기크기를 지정해주는 속성이다.

```JavaScript
function scrub(e) {
  const scrubTime = (e.offsetX / progress.offsetWidth) * video.duration;
  video.currentTime = scrubTime;
}
```

- 재생바에서 클릭하거나 마우스를 누른채 움직인 곳을 계산하여 video.currentTime 을 변경해준다.

```JavaScript
let full = false;
function makeFullScreen() {
  full = !full;
  if (full) {
    player.classList.add("fullScreen");
  } else {
    player.classList.remove("fullScreen");
  }
}
```

- 전체화면을 true, false 값이용해서 classList를 추가 제거하여 css속성을 변경한다.

```JavaScript
video.addEventListener("click", togglePlay);
video.addEventListener("play", updateButton);
video.addEventListener("pause", updateButton);
video.addEventListener("timeupdate", handleProgress);

toggle.addEventListener("click", togglePlay);

skipButtons.forEach((button) => button.addEventListener("click", skip));

ranges.forEach((range) => range.addEventListener("change", handleRangeUpdate));
ranges.forEach((range) =>
  range.addEventListener("mousemove", handleRangeUpdate)
);

let mousedown = false;

progress.addEventListener("click", scrub);
progress.addEventListener("mousemove", (e) => mousedown && scrub(e));
progress.addEventListener("mousedown", () => (mousedown = true));
progress.addEventListener("mouseup", () => (mousedown = false));
```

- 끝!
