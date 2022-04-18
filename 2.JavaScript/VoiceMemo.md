# 음성 기록

> <h2>완성 화면</h2>

![음성인식](https://user-images.githubusercontent.com/82592845/163760690-25915f0e-c5a5-4a65-a6f3-8eb3f3345828.gif)

### - 음성인식, 텍스트로 변환

---

> <h2>HTML</h2>

```jsx
<div class="words" contenteditable></div>
```

- <h3>인식된 문자가 들어갈 div</h3>

---

> <h2>JAVASCRIPT</h2>

```jsx
window.SpeechRecognition =
  window.SpeechRecognition || window.webkitSpeechRecognition;

const recognition = new SpeechRecognition();
recognition.interimResults = true;

let p = document.createElement("p");
const words = document.querySelector(".words");

words.appendChild(p);

recognition.addEventListener("result", (e) => {
  const transcript = Array.from(e.results)
    .map((result) => result[0])
    .map((result) => result.transcript)
    .join("");

  p.textContent = transcript;

  if (e.results[0].isFinal) {
    p = document.createElement("p");
    words.appendChild(p);
  }

  if (transcript.includes("바보")) {
    console.log("🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽");
  }
});

// 계속 다시 실행되게 해준다.
recognition.addEventListener("end", recognition.start);

recognition.start();
```

- interimResults속성은 SpeechRecognition중간 결과를 반환할지 또는 반환 하지 않을지 를 제어한다. 중간 결과는 아직 최종 결과가 아니기 때문이다. [mdn 참고](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition)
- `SpeechRecognition()` 는 오디오인식을 위한 Controller interface다.
- 말을 멈추면 새로운 `paragraph(p태그)`를 생성 한다.
- `appendChild` 로 words 클래스 태그인 div에 p태그를 붙인다.
  - append() 메서드를 활용하면 노드 객체나 DOMString(text)를 사용할 수 있다.
  - 또한 한번에 여러개의 자식요소를 설정할수 있다.
  - appendChild() 메서드는 append 메서드와는 다르게 오직 Node객체만 받을 수 있다.
  - append는 여러개의 노드와 문자를 추가할 수 있는 반면에 한번에 오직 하나의 노드만 추가할 수 있다.
  - 또한 DOMString을 넣을 경우에는 에러가 발생한다.
  - 마지막엔 return 값을 반환한다.
- 오디오가 인식되고 그 데이터가 정제되는 과정, 그리고 아래와 같이 진행하면 단 한번만 실행 된다.

  ```jsx
  const transcript = Array.from(e.results)
    .map((result) => result[0])
    .map((result) => result.transcript)
    .join("");
  console.log(transcript);
  p.textContent = transcript;
  ```

  <img width="331" alt="image" src="https://user-images.githubusercontent.com/82592845/163765486-88b27b8f-3cfe-429d-a1a5-aae1c67bb469.png">
  <img width="323" alt="image" src="https://user-images.githubusercontent.com/82592845/163765661-a798cfb8-be42-4a84-b986-3321283bc0d6.png">
  <img width="332" alt="image" src="https://user-images.githubusercontent.com/82592845/163765743-3b3a8c15-4eac-4658-b72a-1e4380092a6a.png">

  - 위와 같이 인식된 음성데이터가 정제된다.

- 여기까지만 하면 단 한번만 실행이 되기에
  ```jsx
  if (e.results[0].isFinal) {
    p = document.createElement("p");
    words.appendChild(p);
  }
  ```
  - 그러므로 위와 같이 진행하여 계속해서 다시 실행되게 한다.
- 그리고 '바보' 라는 단어가 들어가면 돼지 이모티콘을 출력하게 한다.
  ```jsx
  if (transcript.includes("바보")) {
    console.log("🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽🐽");
  }
  ```
  - includes를 사용하여 포함하고 있다면 `console.log`를 찍는다.
    <img width="339" alt="image" src="https://user-images.githubusercontent.com/82592845/163770517-bb818a37-f097-4ce0-918c-c3bad943ce2b.png">
