# CSS GRID

# Grid

---

> 그리드는 행과 열(2차원)의 레이아웃 시스템이다.

---

### Flex도 좋지만 조금 단순하여 조금 더 복잡한 CSS를 사용하게 해준다.

### 부모요소에 grid를 적용하면 자식요소 모두가 Grid 규칙의 영향을 받게 된다.

### 자식요소들을 Grid Item 그리드 아이템이라 한다.

### 그리드의 행 또는 열을 Grid Track 그리드 트랙이라 한다.

### 그리드의 한칸 한칸을 Grid Cell 그리드 셀 이라 한다. ( Grid Item 그리드 아이템이 들어가는 가상의 틀 )

### Grid Cell 그리드 셀을 구분하는 선은 Grid Line 이라한다.

### Grid Line 그리드 라인의 각 번호를 Grid Number 그리드 번호라 한다.

### Grid Cell 그리드 셀 사이의 간격을 Grid Gap 그리드 갭이라 한다.

### Grid Line 으로 둘러싸인 사각형 영역을 Grid Area 그리드 영역이라 한다. 즉 Grid Cell 그리드 셀의 집합니다.

### Flex 를 사용해봤다면 알고 있듯이 Grid 역시 컨테이너와 아이템에 적용하는 속성 2가지로 나뉜다.

---

## 그리드 사용법을 알아보자

### Grid 를 사용하기 위해선 flex 를 줄때와 같이 `display: grid;` 를 해주면 된다.

---

### grid-template-rows 와 grid-template-columns

- Grid 트랙의 크기들을 지정해주는 속성
- rows와 columns 에서 알 수 있듯이 행과 열의 배치다.
- `grid-template-columns: 1fr 1fr 1fr 1fr 1fr;` 을 적용하면 1:1:1:1:1 인 3개의 열을 만들어준다.

```jsx
<div class="container">
    <div class="a">안녕</div>
    <div class="b">하이</div>
    <div class="c">봉주르</div>
    <div class="d">오하이요</div>
    <div class="e">니하오</div>
</div>

<style>
    .container {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
				grid-template-rows: 1fr 1fr 1f 1fr 1fr;
        height: 50px;
        align-items: center;
        justify-content: center;
        background-color: skyblue;
    }
    .container > div {
        background-color: #7f7f7f;
    }
</style>
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled.png)

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%201.png)

- 이렇게 비율이 1:1:1:1:1 가 된다.
- 비율은 원하는데로 설정이 가능하다.
- 컨테이너의 폭을 조절해도 알아서 비율을 조정해준다.

---

## 이번엔 grid에서 사용가능한 repeat 함수를 알아보자

```php
grid-template-columns: repeat(5, 1fr);
```

- 위의 스타일을 추가하면 `grid-template-columns: 1fr 1fr 1fr 1fr 1fr;` 와 같은 결과를 보여준다.

### ⇒ 즉 repeat 는 반복되는 값을 자동으로 처리해준다. `repeat(반복횟수, 반복값)` 이다.

`repeat(4,2fr,1fr,4fr);` 와 같은 것도 가능하다.

---

### repeat를 더 잘 사용하기 위해 minmax 함수를 알아보자

- min max 에서 알수 있듯이 최솟값, 최댓값이 지정 가능한 함수다!
- 예를 들어 `minmax(50px, auto)` 를 하면 최솟값 50px에 최대값은 auto로 자동으로 늘어나는 것이다.

⇒ 내용의 양이 적어도 최소한 높이는 50px 을 확보하며, 내용이 50px을 넘길 경우에는 알아서 늘어나는 것이다.

```jsx
<div class="container">
  <div class="a" style="background-color: red;">
    1
  </div>
  <div class="b" style="background-color: orange;">
    2
  </div>
  <div class="c" style="background-color: yellow;">
    3
  </div>
  <div class="d" style="background-color: green;">
    4
  </div>
  <div class="e" style="background-color: indigo;">
    5 입니다. 5 입니다. 5 입니다. 5입니다.
  </div>
  <div class="e" style="background-color: purple;">
    6
  </div>
  <div class="e" style="background-color: baige;">
    7
  </div>
  <div class="e" style="background-color: darkgray;">
    8
  </div>
  <div class="e" style="background-color: aliceblue;">
    9
  </div>
</div>
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%202.png)

- container의 5번째 자식요소의 글이 길다. 그냥 `grid-template-columns: repeat(5, 1fr);` 를 적용한다면 이렇게 자신의 요소를 벗어나게된다.
- 그렇다면 `minmax` 를 적용해보자

```jsx
grid-template-columns: repeat(5, minmax(100px, auto));
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%203.png)

- 이렇게 100px 이 넘어도 auto 자동으로 범위를 늘린다.
- 이는 `grid-template-rows` 에서도 똑같이 작동한다.

---

### 이번엔 auto-fill과 auto-fit을 알아보자.

- 이 두가지는 column의 개수를 먼저 정하지 않고 설정된 width나 height 의 너비값이 허용하는 한 최대한 Cell 셀을 채워준다.

### auto-fill

```jsx
grid-template-columns: repeat(auto-fill, minmax(33%, auto));
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%204.png)

- 자식 div 의 갯수를 늘리고 위의 코드를 적용한 것이다.
- 33% 로 설정했기에 한줄에 3개의 셀이 들어가게 된다.
- 그러다 모자르면 13번 옆에를 보면 알 수 있듯 남는 공간이 생긴다.

### 그럼 auto-fit을 사용 하면 어떻게 될까?

- 위의 예제에서는 변화가 없으니 자식 div의 갯수를 줄이고 보자!

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%205.png)

- div를 4개로 줄인 후 `grid-template-columns: repeat(auto-fill, minmax(20%, auto));` 을 적용해 봤다. 보이듯이 한칸이 남는다.

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%206.png)

- `grid-template-columns: repeat(auto-fit minmax(20%, auto));` 을 적용하면 이렇게 빈공간을 채운다!

---

## gap, row-gap, column-gap 을 알아보자!

- 역시 gap 에서 알 수 있듯 간격이다.
- 적용하여 차이를 알아보자.

```jsx
row-gap:20px;
column-gap:20px;
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%207.png)

- 행과 열의 gap이 똑같이 간격이 생긴것을 볼 수 있다.
- 이 간격의 크기에 따라 요소의 크기가 변하기도 한다.

---

## 그리드 형태를 자동으로 정의해주는 grid-auto-columns, grid-auto-rows를 알아보자!

- `grid-template-columns` 와 `grid-template-rows` 를 사용하다보면 통제를 벗어날 때가 있는데 이때 벗어난 트랙의 크기를 지정하는 속성이다.
- 통제를 벗어나는 상황이 언제일까?

```jsx
grid-template-rows: repeat(4, minmax(50px, auto);
```

- 위 코드를 적용한다 생각하면 최소로 설정된 50px을 넘기게 되면 자동으로 늘어나게 될 것이다. 또한 row 수를 알기 때문에 repeat을 설정해줘서 반복한것이다.
- 이때 `mapping` 같은 것을 이용하여 만든다면 row개수를 알수가 없게 된다. 이와같은 상황에 `grid-template-rows` 와 `grid-template-columns` 과 같은 것을 사용하면 `rows` 나 `columns` 의 너비를 잡아준다.

```jsx
grid-template-columns: repeat(3, minmax(100px, auto));
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%208.png)

```jsx
grid-template-columns: repeat(3, minmax(100px, auto));
grid-auto-rows: minmax(50px, auto);
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%209.png)

- 이렇게 rows의 넓이를 잡아줬다.
- 즉 횟수를 지정해줄 필요없이 알아서 처리되는 것이다.

---

## grid-column-start / grid-column-end / grid-columngrid-row-start / grid-row-end / grid-row

- 이 친구들은 Gird 아이템에 적용하는 속성이다. 즉 각 셀의 영역을 지정하는것이다.

![그리드시작.jpg](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/%25E1%2584%2580%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2584%2583%25E1%2585%25B3%25E1%2584%2589%25E1%2585%25B5%25E1%2584%258C%25E1%2585%25A1%25E1%2586%25A8.jpg)

- 위의 그림에선 열 1 ~4 행 1 ~ 4 인것을 확인할 수 있다.
- 이를 이용하여 x 부터 ~ y 까지 영역을 지정해 줄 수 있는 것이다.

```jsx
grid-column-start: 1;
grid-column-end: 3;
grid-row-start: 1;
grid-row-end: 3;
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%2010.png)

- 첫번째 div에 설정해준 것이며, 이런식으로 진행 되는것을 알자!

```jsx
grid-column: 1 / 3;
grid-row: 1 / 3;
```

- 이렇게도 표현이 가능하며, 결과값이 같다.

```jsx
grid-column: 1 / span 3;
grid-row: 1/ span 3;
```

- 이것도 같은 것이다!

---

## 영역 이름으로 설정 하는 방법도 알아보자!

### 각 Grid Area에 이름을 붙인 후 그를 이용하여 배치하는 방법이다.

![영역그리드.jpg](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/%25E1%2584%258B%25E1%2585%25A7%25E1%2586%25BC%25E1%2584%258B%25E1%2585%25A7%25E1%2586%25A8%25E1%2584%2580%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2584%2583%25E1%2585%25B3.jpg)

```jsx
div {
	display: grid;
	grid-template-areas:
		"header header header"
		"   a    main    b   "
		"   .     .      .   "
		"footer footer footer";
}
```

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%2011.png)

- 이렇게 표시가 가능하다.

---

## grid-auto-flow 자동배치! 를 알아보자

- 아이템 자동 배치 흐름을 결정한다.

```jsx
.container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(25%, auto));
    grid-template-rows: repeat(5, minmax(50px,auto));
    grid-auto-flow: row / column / dense / row dense / column dense ;
}
.b { grid-column: auto / span 3; }
.e { grid-column: auto / span 3; }
.g { grid-column: auto / span 2; }
```

- 2 5 7 번째 div 에 셀을 지정해줬다.
- `grid-auto-flow` 종류를 하나하나 알아보자

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%2012.png)

- row

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%2013.png)

- column

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%2014.png)

- dense

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%2015.png)

- row dense

![Untitled](CSS%20GRID%208d539292ebee41fb86a1c79c8c734b6b/Untitled%2016.png)

- column dense

---

### align-items, justify-items 이건 알고 있다고 생각한다.

### place-items, align-content(**아이템 그룹 세로 정렬)**, justify-content(**아이템 그룹 가로 정렬), place-content, align-self(개별 아이템 세로 정렬), justify-self(개별 아이템 가로 정렬), place-self, order (배치순서), z-index(Z축 정렬) 이 것들은 직접 하나씩 사용해보면서 알아보자**

### 참고 - 1분코딩님 ( [링크](https://studiomeal.com/archives/533) [https://studiomeal.com/archives/533](https://studiomeal.com/archives/533) )

```jsx

```
