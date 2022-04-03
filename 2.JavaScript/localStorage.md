> 목표

![체크리스트](https://user-images.githubusercontent.com/82592845/161432427-8169bfb3-586d-4400-8e1b-475d8fa3b351.gif)

> event delegation 하는법 & localStorage사용하여 새로고침해도 변하지 않는 to-do list 같은것 만들기

### 1. HTML

```jsx
<div class="wrapper">
  <h2>LOCAL TAPAS</h2>
  <p></p>
  <ul class="plates">
    <li>Loading Tapas...</li>
  </ul>
  <form class="add-items">
    <input type="text" name="item" placeholder="Item Name" required />
    <input type="submit" value="+ Add Item" />
  </form>
</div>
```

### 2. CSS

```jsx
html {
  box-sizing: border-box;
  background: url("oh-la-la.jpeg") center no-repeat;
  background-size: cover;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-family: Futura, "Trebuchet MS", Arial, sans-serif;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

// ----------- 아이콘 ----------- //
svg {
  fill: white;
  background: rgba(0, 0, 0, 0.1);
  padding: 20px;
  border-radius: 50%;
  width: 200px;
  margin-bottom: 50px;
}

// ----------- form, list wrapper ----------- //
.wrapper {
  padding: 20px;
  max-width: 350px;
  background: rgba(255, 255, 255, 0.95);
  box-shadow: 0 0 0 10px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
  margin: 0;
  font-weight: 200;
}

.plates {
  margin: 0;
  padding: 0;
  text-align: left;
  list-style: none;
}

.plates li {
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  padding: 10px 0;
  font-weight: 100;
  display: flex;
}

.plates label {
  flex: 1;
  cursor: pointer;
}

.plates input {
  display: none;
}

/* 아래 두개로 체크전 체크 후 를 만든다. */
.plates input + label:before {
  content: "⬜️";
  margin-right: 10px;
}

.plates input:checked + label:before {
  content: "🌮";
}

.add-items {
  margin-top: 20px;
}

.add-items input {
  padding: 10px;
  outline: 0;
  border: 1px solid rgba(0, 0, 0, 0.1);
}
```

### 3. JavaScript

```jsx
const addItems = document.querySelector(".add-items");
const itemsList = document.querySelector(".plates");
const items = JSON.parse(localStorage.getItem("items")) || [];
```

- querySelector로 불러오기

```jsx
function addItem(e) {
  e.preventDefault();
  const text = this.querySelector("[name=item]").value;
  const item = {
    text,
    done: false,
  };
  items.push(item);

  populateList(items, itemsList);
  localStorage.setItem("items", JSON.stringify(items));
  this.reset();
}
```

- `querySelector(”[name=item]”)` 이 `<input type="text" name="item" placeholder="Item Name" required />` 를 불러 올 수 있는것이 신기했다.
- 아무튼 value 값을 받아서 text에 삽입후 item객체에 넣어준다.
- text=text ⇒ text, 간편하게 사용
- 체크 여부 판독해줄 done
- items에 생성된 item을 push 해준후 리스트 생성해주는 populateList함수를 실행시킨다.
- `localStorage.setItem("items", JSON.stringify(items));` items라는 키값으로 items를 넣어준다.
- 그리고 리셋 시켜 input을 비워준다.

```jsx
function populateList(plates = [], platesList) {
  platesList.innerHTML = plates
    .map((plate, i) => {
      return `
          <li>
              <input type="checkbox" data-index=${i} id="item${i}" ${
        plate.done ? "checked" : ""
      } />
              <label for ="item${i}">${plate.text}</label>
          </li>
      `;
    })
    .join("");
}
```

- 화면에 보여지는 list가 추가되는 부분이다.
- 매개변수로 들어온 plates를 map으로 값들을 하나하나 return 해주는데 li 로 생성해주는 것이다.
- plates를 mapping 한것들 innerHTML으로 삽입해주는데, 이때 마지막 `.join(””)` 없이 하게 되면 아래 사진에서 처럼 배열안의 , 가 그대로 나오게 된다. 때문에 .join(””)을 해줘야 한다. ( , 없애고 나온다!)

<img width="631" alt="image" src="https://user-images.githubusercontent.com/82592845/161435730-8f30fb63-5cff-4daf-abef-bba05f2b78ab.png">

<br />

```jsx
function toggleDone(e) {
  if (!e.target.matches("input")) return;
  const el = e.target;
  const index = el.dataset.index;
  items[index].done = !items[index].done;
  localStorage.setItem("items", JSON.stringify(items));
  populateList(items, itemsList);
}
```

- 클릭이벤트 발생지점이 plates요소안의 input이 return 시킨다.
- index값을 받아서 배열에서 해당 인덱스의 done값을 반대의 값으로 다시 삽입하여 localStorage에 저장한다.
