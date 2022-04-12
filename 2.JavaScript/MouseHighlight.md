> <h2>목표!!</h2>

> <h3>말 그대로 형광팬! 마우스 가져대면 하이라이터 효과가 있게 하기!</h3>

## 1. 사용할 요소 변수에 넣기

```html
const triggers = document.querySelectorAll('a')
```

## 2. js를 사용해 highlight를 사용할 span요소를 만들고, 나중에 길이, 너비, 위치를 제어한다.

```html
const highlight = document.createElement('span')
highlight.classList.add('highlight') document.body.append(highlight)
```

- 하이라이트에 `span` 을 사용한다.
- classList.add()를 이용해 css에 있는 highlight를 추가해준다.
- append()를 사용해서 body에 highlight를 추가해준다.
  - .append()는 선택된 요소의 마지막에 새로운 요소나 콘텐츠를 추가한다.
  - $(target).append(source)
  - [https://araikuma.tistory.com/609](https://araikuma.tistory.com/609)

## 3. 모든 요소에 mouseenter를 바인딩한다.

```html
function highlightLink() { } // 미완성 triggers.forEach(a => {
a.addEventListener('mouseenter', highlightLink) })
```

- triggers가 id를 가진 모두인것 같다.(?)

## 4. highlightLink() 기능을 만드는 과정

### 1. 각 요소의 좌표와 길이 및 너비 등을 얻는다.

```html
function highlightLink () { const linkCoords = this.getBoundingClientRect()
console.log(linkCoords); }
```

`Element.getBoundingClientRect()`

메서드는 DOMRect요소의 크기와 뷰포트에 상대적인 위치에 대한 정보를 제공 하는 객체를 반환합니다 .

![스크린샷 2021-09-24 오후 3.52.39.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b1aaf9da-2fa3-4d8a-afe7-0152fd5dbc69/스크린샷_2021-09-24_오후_3.52.39.png)

### 2. linkCorrds를 이용해서 넓이와 높이를 설정한다.

```html
function highlightLink () { const linkCoords = this.getBoundingClientRect();
console.log(linkCoords); const coords = { width: linkCoords.width, height:
linkCoords.height, top:linkCoords.top, left:linkCoords.left, };
highlight.style.width = `${coords.width}px` highlight.style.height =
`${coords.height}px` highlight.style.top = `${coords.top}px`
highlight.style.left = `${coords.left}px` }
```

- 뷰포트에 따라 변화하는 값을 넣어준다.
- top , left를 지정해주지 않으면 상단에 왼쪽 상단에 위치한다.

### 3. 여기까지 하면 스크롤을 내렸을때 이렇게 이상 징후가 생기기 때문에 top left 에 움직인 스크롤 값을 더해준다.

![스크린샷 2021-09-24 오후 3.59.11.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/175e606f-c904-4ab2-a17f-43dd41ed35cb/스크린샷_2021-09-24_오후_3.59.11.png)

```html
function highlightLink () { ... const coords = { width: linkCoords.width,
height: linkCoords.height, top:linkCoords.top + window.scrollY,
left:linkCoords.left + window.scrollX, }; ... }
```

# 완성!
