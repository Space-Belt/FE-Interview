# 비디오 속도 제어

<img alt="image" src="https://user-images.githubusercontent.com/82592845/163939707-0c153981-4c52-48ea-a02c-e09a59529d29.png">

- 동영상 옆의 바에 마우스를 움직여서 속도를 제어

---

## 과정 알아보기

> <h3>HTML</h3>

```jsx
<div class="wrapper">
  <video
    class="flex"
    width="765"
    height="430"
    src="http://clips.vorwaerts-gmbh.de/VfE_html5.mp4"
    loop
    controls
  ></video>
  <div class="speed">
    <div class="speed-bar">1×</div>
  </div>
</div>
```

- video 태그의 속성이 loop, controls
  - loop는 boolean 속성이며, loop가 설정되면 동영상이 끝나면 다시 초기로 돌아간다.
  - controls는 소리 조절, 동영상 탐색, 일시 정지/재시작이 가능한 컨트롤러를 제공한다.

```jsx
<script>
  const speed = document.querySelector(".speed");
  const bar = speed.querySelector(".speed-bar");
  const video = document.querySelector(".flex");

  function handleMove(e) {
    const y = e.pageY - this.offsetTop;
    const percent = y / this.offsetHeight;
    const min = 0.4;
    const max = 4;
    const height = Math.round(percent * 100) + "%";
    const playbackRate = percent * (max - min) + min;
    bar.style.height = height;
    bar.textContent = playbackRate.toFixed(2) + "×";
    video.playbackRate = playbackRate;
  }

  speed.addEventListener("mousemove", handleMove);
</script>
```

- 필요한 요소들을 컨트롤하기 위해 querySelector를 이용해 가져온다.
- 페이지 상의 Y값, 해당 요소의 위의 Y좌표값, 해당요소의 높이 를 이용하여 컨트롤 하고 비율을 toFixed(2) 를 이용해 소수점 2 자리까지만 표시되게 한다.
