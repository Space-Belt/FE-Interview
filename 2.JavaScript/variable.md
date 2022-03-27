## variable screen control

```html
<div class="controls">
  <label for="spacing">Spacing:</label>
  <input
    id="spacing"
    type="range"
    name="spacing"
    min="10"
    max="200"
    value="10"
    data-sizing="px"
  />

  <label for="blur">Blur:</label>
  <input
    id="blur"
    type="range"
    name="blur"
    min="0"
    max="25"
    value="10"
    data-sizing="px"
  />

  <label for="radius">Radius:</label>
  <input
    id="radius"
    type="range"
    name="radius"
    min="0"
    max="50"
    value="50"
    data-sizing="px"
  />

  <label for="width">width:</label>
  <input
    id="width"
    type="range"
    name="width"
    min="0"
    max="1000"
    value="500"
    data-sizing="px"
  />

  <label for="base">Base Color</label>
  <input id="base" type="color" name="base" value="#ffc600" />
</div>
```

1. input 과 label을 for="spacing" id="spacing" 로 묶는다.
   - <label for="값"> input 태그를 제어하여 상태를 변경하게 도와주는 태그
   - 클릭의 유효범위가 되기도 한다. (checkbox)의 경우
   - input 태의 min , max 로 최소최대값 정해준다. value로 기본값을 설정하낟.
   - data-sizing으로 단위를 서렂ㅇ해준다.

```html
:root { --base: #ffc600; --spacing: 10px; --blur: 10px; --width: 0px; --radius:
0px; } img { (--spacing); /* css 에서는 (--?) 로 한다. */ background:
var(--base); filter: blur(var(--blur)); border-radius: var(--radius); width:
var(--width); } h2 { color: yellow; } .hl { color: var(--base); } body {
text-align: center; background: #193549; color: white; font-family: 'helvetica
neue', sans-serif; font-weight: 100; font-size: 50px; } .controls {
margin-bottom: 50px; color: red; } input { width: 100px; }
```

### CSS

- :root 란?
  - :root 의사클래스는 문서 트리의 루트 요소를 선택한다. 가령 HTML 문서의 루트 요소는 <html> 요소이므로, :root와 html은 같다.
- **`var()`** 함수는 [사용자 지정 속성 (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/--*), 또는 "CSS 변수"의 값을 다른 속성의 값으로 지정할 때 사용합니다.
  - `var(--header-color, blue);`Copy to Clipboard
  - `var()` 함수는 값이 아닌 속성 이름, 선택자 등 다른 곳에 사용할 수 없습니다. 시도할 경우 유효하지 않은 구문이 되거나, 변수와 관계없는 값이 됩니다.
  - [https://developer.mozilla.org/ko/docs/Web/CSS/var()](<https://developer.mozilla.org/ko/docs/Web/CSS/var()>)

## Script

```html
<script>
  const inputs = document.querySelectorAll(".controls input");

  function handleUpdate() {
    console.log(this); // this = 선택된 input
    const suffix = this.dataset.sizing || ""; //색은 sizing이 없기 떄문에 없는것도 해줘야한다.
    document.documentElement.style.setProperty(
      `--${this.name}`,
      this.value + suffix
    );
  }

  inputs.forEach((input) => input.addEventListener("change", handleUpdate));
  inputs.forEach((input) => input.addEventListener("mousemove", handleUpdate));
</script>
```

1. inputs 변수에 .controls에 있는 input 을 다 넣어준다.
2. handleUpdate이름의 함수를 만든다.

   this(상위의).dataset.sizing data-sizing을 넣는다 px 과 색

3. document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);

   읽기 전용 속성은 문서의 루트 요소를 나타내는 Element를 반환합니다. HTML 문서를 예로 들면 <html> 요소를 반환합니다. 해당 문서의 루트를 가리킨다.

   예 ) ( —width, 15 + px);

4. inputs.에 change( 변했을때, handleupdate()삽입);
5. inputs 에 mousemove (마우스 움직였을때, handleUpdate()삽입)
