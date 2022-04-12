> <h3>목표</h3>

> <h3>Scroll 되면서 slidein 되는 사진들을 만들어보자!</h3>

## 일단 슬라이드는 css로 구성되어있다.

```jsx
slide-in { opacity: 0; transition: all 0.5s; } .align-left.slide-in { transform:
translateX(-30%) scale(0.95); } .align-right.slide-in { transform:
translateX(30%) scale(0.95); } .slide-in.active { opacity: 1; transform:
translateX(0%) scale(1); }
```

slide-in 가 active되면서 opacity 와 translateX(위치), scale(크기)가 변화된다.

## 1. slide-in을 부여한 클래스를 사용하여 모든 이미지를 선택한다.

<aside>
💡 const sliderImages = document.querySelectorAll(".slide-in");

</aside>

## 2. scroll 이벤트는 너무 많이 일어나서 성능 문제를 일으킬수 있기 때문에 debounce함수를 만들어서 controll 해준다.

```jsx
function debounce(func, wait = 20, immediate = true) { var timeout; return
function () { var context = this, args = arguments; var later = function () {
timeout = null; if (!immediate) func.apply(context, args); }; var callNow =
immediate && !timeout; clearTimeout(timeout); timeout = setTimeout(later, wait);
if (callNow) func.apply(context, args); }; }
```

- 하는 일은 어떤 함수가 제공되고 어떤 wait간격이 설정되어 있든 간에 전달된 함수가 x초마다 한 번씩 실행되도록 하는 것입니다. 여기서 x는 wait밀리초 단위 의 간격

## 3. scroll 되는정도를 체크하고 조건이 맞으면 active시켜서 슬라이드 기능 실행시킨다.

```jsx
function checkSlide() { sliderImages.forEach(sliderImage => { // 이미지 반을
넘었을때 // (window.scrollY + window.innerHeight) 내가 내린 스크롤 값 + 화면에
보여지는 높이 const slideInAt = (window.scrollY + window.innerHeight) -
sliderImage.height / 2; // bottom of the image // sliderImage.offsetTop 사진
제일 위를 기준으로 페이지의 최정상까지 // sliderImage.offsetTop +
sliderImage.height 그러므로 이미지 하단까지의 길이를 알수 있다. const
imageBottom = sliderImage.offsetTop + sliderImage.height; // 이미지높이의 반을
넘었을떄 const isHalfShown = slideInAt > sliderImage.offsetTop; //
스크롤내린만큼이 이미지의 바닥부분보다 작을때 const isNotScrolledPast =
window.scrollY < imageBottom; if (isHalfShown && isNotScrolledPast) {
sliderImage.classList.add('active'); } else {
sliderImage.classList.remove('active'); } }); }
```
