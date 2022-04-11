# Text Reader( 텍스트 읽어주는 앱 )

<img alt="image" src="https://user-images.githubusercontent.com/82592845/162675669-d85365c5-2831-44d4-9797-2625e84be25d.png">

> <h2>목표</h2>

- Text 입력창에 있는 글을 여러 사람의 목소리와 발음으로 읽어주도록 해보자!
- 읽는 속도와 음정도 조절해보자!

---

> <h2>HTML</h2>

```jsx
<div class="voiceinator">
  <h1>The Voiceinator 5000</h1>
  <select name="voice" id="voices">
    <option value="">Select A Voice</option>
  </select>
  <label for="rate">Rate:</label>
  <input name="rate" type="range" min="0" max="3" value="1" step="0.1">
  <label for="pitch">Pitch:</label>
  <input name="pitch" type="range" min="0" max="2" step="0.1">
  <textarea name="text">Hello! I love JavaScript 👍</textarea>
  <button id="stop">Stop!</button>
  <button id="speak">Speak</button>
</div>
```

- 읽는 속도, 음정은 input의 range type으로 설정하여 조절한다.
- 글이 들어갈 곳엔 textarea를 사용하고 기본적으로 Hello! I love JavaScript 👍 가 나오도록 한다.

---

> <h2>CSS</h2>

```css
html {
  font-size: 10px;
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
  font-family: sans-serif;
  background-color: #3bc1ac;
  display: flex;
  min-height: 100vh;
  align-items: center;

  background-image: radial-gradient(
      circle at 100% 150%,
      #3bc1ac 24%,
      #42d2bb 25%,
      #42d2bb 28%,
      #3bc1ac 29%,
      #3bc1ac 36%,
      #42d2bb 36%,
      #42d2bb 40%,
      transparent 40%,
      transparent
    ), radial-gradient(
      circle at 0 150%,
      #3bc1ac 24%,
      #42d2bb 25%,
      #42d2bb 28%,
      #3bc1ac 29%,
      #3bc1ac 36%,
      #42d2bb 36%,
      #42d2bb 40%,
      transparent 40%,
      transparent
    ), radial-gradient(
      circle at 50% 100%,
      #42d2bb 10%,
      #3bc1ac 11%,
      #3bc1ac 23%,
      #42d2bb 24%,
      #42d2bb 30%,
      #3bc1ac 31%,
      #3bc1ac 43%,
      #42d2bb 44%,
      #42d2bb 50%,
      #3bc1ac 51%,
      #3bc1ac 63%,
      #42d2bb 64%,
      #42d2bb 71%,
      transparent 71%,
      transparent
    ), radial-gradient(
      circle at 100% 50%,
      #42d2bb 5%,
      #3bc1ac 6%,
      #3bc1ac 15%,
      #42d2bb 16%,
      #42d2bb 20%,
      #3bc1ac 21%,
      #3bc1ac 30%,
      #42d2bb 31%,
      #42d2bb 35%,
      #3bc1ac 36%,
      #3bc1ac 45%,
      #42d2bb 46%,
      #42d2bb 49%,
      transparent 50%,
      transparent
    ), radial-gradient(circle at 0 50%, #42d2bb 5%, #3bc1ac 6%, #3bc1ac 15%, #42d2bb
        16%, #42d2bb 20%, #3bc1ac 21%, #3bc1ac 30%, #42d2bb 31%, #42d2bb 35%, #3bc1ac
        36%, #3bc1ac 45%, #42d2bb 46%, #42d2bb 49%, transparent 50%, transparent);
  background-size: 100px 50px;
}

.voiceinator {
  padding: 2rem;
  width: 50rem;
  margin: 0 auto;
  border-radius: 1rem;
  position: relative;
  background: white;
  overflow: hidden;
  z-index: 1;
  box-shadow: 0 0 5px 5px rgba(0, 0, 0, 0.1);
}

h1 {
  width: calc(100% + 4rem);
  margin: -2rem 0 2rem -2rem;
  padding: 0.5rem;
  background: #ffc600;
  border-bottom: 5px solid #f3c010;
  text-align: center;
  font-size: 5rem;
  font-weight: 100;
  font-family: "Pacifico", cursive;
  text-shadow: 3px 3px 0 #f3c010;
}

.voiceinator input,
.voiceinator button,
.voiceinator select,
.voiceinator textarea {
  width: 100%;
  display: block;
  margin: 10px 0;
  padding: 10px;
  border: 0;
  font-size: 2rem;
  background: #f7f7f7;
  outline: 0;
}

textarea {
  height: 20rem;
}

.voiceinator button {
  background: #ffc600;
  border: 0;
  width: 49%;
  float: left;
  font-family: "Pacifico", cursive;
  margin-bottom: 0;
  font-size: 2rem;
  border-bottom: 5px solid #f3c010;
  cursor: pointer;
  position: relative;
}

.voiceinator button:active {
  top: 2px;
}

.voiceinator button:nth-of-type(1) {
  margin-right: 2%;
}
```

- 배경은 직접 css로 만든것이다 ( [radial-gradient 알아보기](https://developer.mozilla.org/en-US/docs/Web/CSS/gradient/radial-gradient) )

---

> <h2>JAVASCRIPT</h2>

<h3>1. 메세지 읽어줄 인스턴트개체 생성 </h3>

```jsx
const msg = new SpeechSynthesisUtterance();
```

<h3>2. 목소리 영어(en)만 불러오기 </h3>

```jsx
function populateVoices() {
  voices = this.getVoices();
  //const voiceOptions = voices
  voicesDropdown.innerHTML = voices
    .filter((voice) => voice.lang.includes("en"))
    .map(
      (voice) =>
        `<option value="${voice.name}">${voice.name} (${voice.lang})</option>`
    )
    .join("");
  //voicesDropdown.innerHTML = voiceOptions; 이렇게 하거나 위에처럼 하나로 합치거나
}
```

- 총 67개의 목소리가 있음
- this 는 window.speechSynthesis 다. ([알아보기](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis))
- filter로 영어만 가져오기
- mapping 하고 join 으로 "," 없애준다.

<h3>3. 목소리 선택 </h3>

```jsx
function setVoice() {
  msg.voice = voices.find((voice) => voice.name === this.value);
  toggle();
}
```

- msg.voice에 선택한 옵션을

<h3>4. 바뀐 것들을 바뀐곳에 삽입해준다. </h3>

```jsx
function setOption() {
  msg[this.name] = this.value;
  toggle();
}
```

- this.name 은 property
- this.value 는 들어가있는 값이다.

<h3>5. 바뀔 때마다 초기화를 시켜준다. </h3>

```jsx
function toggle(startOver = true) {
  speechSynthesis.cancel();
  if (startOver) {
    speechSynthesis.speak(msg);
  }
}
```

- speechSynthesis.cancel(); 하던 것들을 중단한다.
- 그리고 변화가 감지되면 바로 msg에 있는것들을 읽어준다.

```jsx
let voices = [];
const voicesDropdown = document.querySelector('[name="voice"]');
const options = document.querySelectorAll('[type="range"], [name="text"]');
const speakButton = document.querySelector("#speak");
const stopButton = document.querySelector("#stop");

msg.text = document.querySelector('[name="text"]').value;

speechSynthesis.addEventListener("voiceschanged", populateVoices);
voicesDropdown.addEventListener("change", setVoice);
options.forEach((option) => option.addEventListener("change", setOption));
speakButton.addEventListener("click", toggle);
stopButton.addEventListener("click", () => toggle(false));
```

- HTML 태그와의 연결, eventListener
