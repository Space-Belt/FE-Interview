# CSS Flex

# Flex

---

- 기본적으로 좀 많이 사용해봤기에 간단하게 정리할 것이다.
- flex 를 사용하면 float, inline-block 을 이용한 옛 방식보다 훨씬 편한 기능이 많다.
- 컨테이너, 아이템에 적용하는 속성 2가지로 나뉜다.

---

## 적용 방법

---

### display: flex; 으로 컨테이너에 적용 가능하다.

- 기본적으로 내용물의 width 만큼만 적용되어 정렬된다. (inline 요소같이)
- 가로 방향(row)로 적용 배치된다.
- height가 알아서 컨테이너의 높이만큼 늘어난다.
- inline-flex도 존재하는데 block과 inline-block과의 관계와 같다. ( inline-block처럼 동작 )

---

### 아이템들이 배치된 방향의 축 = 메인축 (Main Axis)

메인축과 수직인 축 = 수직축 or 교차축 (Cross Axis) 라 한다.

---

### Flex-direction (배치방향을 설정 가능하다.)

- row ———>
- column (아래로)
- row-reverse / column-reverse. 둘다 원래 방향의 역방향으로 진행되는것

---

### Flex-wrap ( 줄넘김 처리 )

- 아이템이 삐져나갈때 어떻게 아이템끼리 줄바꿈을 할지 안할지를 정하는것이다.
- nowrap ( 그냥 삐져 나간다 )
- wrap ( 다음줄로 넘어간다 )
- wrap-reverse ( 다음줄이 아닌 역으로 위로 올라간다. ) 아이템 역순 배치

---

### Flex-flow

- flex-direction 과 flex-wrap을 한번에 지정가능한 것이다.
- `flex-flow: row wrap:` ⇒ `flex-direction: row; flex-wrap: wrap;` 같은것이다. 한 칸 떼고 사용
- justify ⇒ 메인축, align ⇒ 수직축 —————— 메인축이면 —-ㅣ—-ㅣ—-ㅣ—ㅣ— ( l 이 수직축)

---

### justify-content ( 메인축 방향 정렬 )

- flex-start ⇒ 시작점으로 정렬 row (가로) 때는 왼쪽 , column(세로) 때는 위
- flex-end ⇒ 끝점으로 정렬 row 오른쪽 column 아래
- center ⇒ 가운데 정렬
- space-between ⇒ 아이템 사이에 똑같은 간격을 만들어줌.
- space-around ⇒ 아이템 둘레에 균일한 간격을 만들어줌.
- space-evenly ⇒ 아이템 사이뿐만 아니라 양끝에도 균일한 간격을 만들어준다.

---

### align-items ( 수직축 방향 정렬 )

- 수직축 방향으로 아이템 정렬하는 속성이다.
- stretch ⇒ 아이템들이 수직축 방향으로 끝까지 늘어난다.
- flex-start ⇒ 아이템들을 시작점으로 정렬한다. row 는 위, column 는 오른쪽
- center ⇒ 가운데 정렬
- baseline ⇒ 아이템들을 텍스트 베이스라인 기준으로 정렬 ( 글씨의 베이스라인 기준 )

---

### align-content(여러 행 정렬)

- `flex-wrap: wrap;` 설정 상태에서, 아이템들의 행이 2 줄 이상 됐을때 수직축 방향 정렬을 결정한다.

---

### flex-basis ( 유연한 박스의 기본 영역 )

- `flex-basis: 50px;` 를 지정하면 50px 이 안되는 요소는 50px로 늘어난다.
- `width: 100px;` 로 설정한다면 뭐든 100px 로 맞춰진다. 영역을 넘어가야하는 것이 있다면 CSS에 `word-wrap: break-word;` 를 적용해주면 된다.
- 동시에 설정 가능하다.

---

### flex-grow ( 유연하게 늘리기 )

- 아이템이 `flex-basis` 의 값보다 커질 수 있는지를 결정하는 속성이다.
- 숫자값이 들어가며, 0보다 큰수여야한다. (default 0)
- `flex-basis` 를 제외한 **여백** 부분을 `flex-grow`에 지정된 숫자의 비율로 나눠가진다.
- 여백 기준이다!

---

### flex-shrink ( 유연하게 줄이기 )

- `flex-grow` 와 쌍을 이루며 `flex-basis` 의 값보다 작아질 수 있는지를 정한다.
- 숫자값을 받으며 0 보다 큰 값이어야 한다.
- 기본값이 1 이다. 따라서 세팅하지 않아도 `flex-basis` 보다 작아질 수 있다.

---

### flex

- `flex-grow, flex-shrink, flex-basis` 를 한번에 사용가능하다.
- `flex: 1;` 을 설정하면 grow, shrink 가 1; basis는 0%가 된다.
- `flex: 1 1 auto;` 는 grow→ shrink → basis 순이다.
- `flex: 1 500px;` 은 1 1 500px 로 설정되는 것이다.

---

### align-self(수직축으로 아이템 정렬)

- `align-items` 의 아이템 버전이다. 해당 아이템의 수직축 방향 정렬!
- stretch, flex-start, flex-end, center, baseline 이 있다.

---

### order (배치 순서)

- 시각적 나열 순서를 결정한다.
- html 구조를 바꾸는것이 아니고 보이는것만 바꾼것

---

### z-index ( position 의 z-index 와 같다 )
