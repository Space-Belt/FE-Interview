> 목표

![체크리스트](https://user-images.githubusercontent.com/82592845/161432427-8169bfb3-586d-4400-8e1b-475d8fa3b351.gif)

> event delegation 하는법 & localStorage사용하여 새로고침해도 변하지 않는 to-do list 같은것 만들기

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>LocalStorage</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <svg> // 생략
    </svg>

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

    <script>
      // <form class="add-items"> </form> 영역
      const addItems = document.querySelector(".add-items");
      //  <ul class="plates"> </ul> 영역
      const itemsList = document.querySelector(".plates");
      // 전체를 이 배열에 넣는다.
      // 아래 진행 하다가 페이지가 로드됐을때 저장됐던것들을 띄우기 위해서 하는 작업
      // localStorage에서 정보를 가져오는걸 시도해보고 없으면   [];
      const items = JSON.parse(localStorage.getItem("items")) || [];

      function addItem(e) {
        /*
    console.log('Hello');
    콘솔창에서 preserve log 를 하면
    Hello    /  Navigated to http://127.0.0.1:5500/15LocalStorageandEventDelegation/delegation.html?item=Fish
                        일하고 있는 url 도 알려준다.
     이 과정으로 페이지가 새로고침이 되고있다는것을 알수 있고 이를 방지하기 위해
     form 의 default가 reloading 또는 send data (to server) 이기 때문에 밑에서
    */
        e.preventDefault(); // 페이지 reloading 안되게끔.
        // 이제 입력했을때 object에 넣는 과정을 만든다.

        // 2. 이제 여기에 어떻게 텍스트를 가져오는지 만든다.
        // <input type="text" name="item" placeholder="Item Name" required>
        // const text = document.querySelector('[name=item]') 도 가능하지만
        // this 는 전체 form 태그가 되기 떄문에 가능하다. [name 이 item인 친구]
        const text = this.querySelector("[name=item]").value; // value를 붙이는 이유를 잘 모르겠다.
        console.log(this.querySelector("[name=item]"));
        // 1. 아이템을 하나 만든다
        const item = {
          //text: 'Item Name',     text: text,  이렇게 가능한 이유는 무엇인가 esx(?) shorthandproperty로
          text,
          done: false, // 디폴트로 체크가 되지 않게 해준다.
        };
        // 4. items 배열에 인풋한것을 넣는다.
        console.log(item);
        items.push(item);

        // populateList 만든후 하면 화면에 인풋하는데로 보여진다.
        populateList(items, itemsList);

        // 로컬저장소 local storage에 저장하기 위해서 하는것
        //localStorage.setItem('items', items); // 이렇게하면 string만 받아들이는 속성때문에 Object, object로 저장된다.
        // JSON.parse 하면 다시 Object object 로
        localStorage.setItem("items", JSON.stringify(items)); // JSON.stringify(items) 사용으로 바꿔준다. ~~~~ 찾아보기 ~~~~

        // 3. 쳤을때 인풋창을 리셋한다. form이 가진 reset 메소드
        this.reset();
      }

      // 이부분 어렵다. 8:00 ~
      // html에 보여지는 리스트를 만들기 위한 함수
      // plates= items 로 해도되지만 plates = [empty object]로 한다. 이유는 ? 아무것도 입력했을때 아무것도 하지 않고 다시 roop(반복?) 해주기떄문
      function populateList(plates = [], platesList) {
        // 필요한 것은 2가지  list와  item에 restore하는것
        platesList.innerHTML = plates
          .map((plate, i) => {
            //plate = item   i= index
            // id =item${i} , for=item${i} 로 서로 연관관계를 맺어준다.
            // input에 checked를 해놓으면 무조건 적으로 체크가 되기떄문에 checked => ${plate.done ? 'checked' : ''} item.done
            return `
            <li>
                <input type="checkbox" data-index=${i} id="item${i}" ${
              plate.done ? "checked" : ""
            } />
                <label for="item${i}">${plate.text}</label>
            </li>
        `;
          })
          .join(""); // 어레이를 스트링으로 만들어주는것    map 은 배열을 리턴하고  innerHTML을 사용하기떄문
      }

      // 새로고침했을때도 체크가 남아있도록  이부분은 이해가 좀 어렵다.
      function toggleDone(e) {
        //4.  이렇게 콘솔을 띄워보면 클릭했을때 2개의 마우스이벤틀핸들러가 보인다.
        // console.log(e);

        //5.  target을 이용하여 뭐가 클릭됐는지 확인하고 하나만 클릭하도록 한다.
        if (!e.target.matches("input")) return; // skip this unless it's an input 인풋이 아니면 스킵해라
        //console.log(e.target); // label 과 list 가 따로따로 인식되서 2번씩 뜨는것을 볼수있는데 해결하기 위해서

        //6.  이제 아이템 배열에 가서 체크된것을 찾고 set the done to be true or false depending on what states in
        // 인덱스 번호를 줬던것으로 배열에서 어떤 인자인지 알수있으니 그것을 이용해서 해결
        const el = e.target;
        //console.log(el.dataset.index);      //선택한 리스트의 인덱스 번호를 콘솔로그에 띄운다.
        const index = el.dataset.index; // 인덱스에 el.dataset.index
        // 앞에것이 true 면 false 가 되도록 만들어준다.  change the property 이과정을 왜하는지모르곘다.
        items[index].done = !items[index].done;
        localStorage.setItem("items", JSON.stringify(items));
        populateList(items, itemsList);
      }

      // submit 제출  거의 모든것을 제출할수 있는것 같다.   ( 한번 찾아서 정리 )
      addItems.addEventListener("submit", addItem);

      // 눌렀을때 어떤 효과를 주는 과정
      // 1. 처음에 이렇게 하면 리스트를 눌렀을때는 아무것도 발생하지 않고 input공간을 눌렀을때만 작동한다.
      // const checkBoxes = document.querySelectorAll('input');
      // checkBoxes.forEach(input => input.addEventListener('click', () => alert('hi')));
      // 이유는 리스트가 아직 존재하지 않기떄문이다.

      // 2.  그래서 populateList(items, itemsList); 를 먼저 올린다.
      // 하지만 이렇게 하고 새로운 것을 input 하면 다시 작동하지 않는다.
      // 그 이유는 다시 인풋을 하면 이것들을 이미 작동한 후에 다시 인풋이 되기 떄문에 작동하지 않는다.
      // c => a   => b (input) => ? 과정을 해야하는데 다시 인풋하면 이벤트를 붙여주는것이 없다.
      // populateList(items, itemsList);
      // const checkBoxes = document.querySelectorAll('input');
      // checkBoxes.forEach(input => input.addEventListener('click', () => alert('hi')));

      // 3. html을 보면 ul 가 다 가지고 있는데 부모요소니 여기에 넣어주면 자식요소인 li에 자동으로 상속받는다. ??
      // const itemsList = document.querySelector('.plates');
      itemsList.addEventListener("click", toggleDone);

      populateList(items, itemsList);

      // 페이지가 로드됐을때 저장됐던것들을 띄우기 위해서 하는 작업
      // populateList(items, itemsList);

    </script>
  </body>
</html>

```
