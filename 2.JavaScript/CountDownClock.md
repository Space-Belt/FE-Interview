> <h2>목표!!</h2>

> 시간 nav
> 카운트다운 타이머를 만들자
> 타이머가 다되면 몇 시가 될 것인지만들기!

```jsx
function timer(seconds) {
  setInterval(function () {
    seconds--;
  }, 1000);
}
```

- 이렇게 자주 하는데 setInterval은 오류가 생기거나 작동하지 않을때가 있으므로 이렇게 하지 않는다.

```jsx
function timer(seconds) {
  setInterval(() => {
    // then - now 는 now+seconds*1000 - now
    const secondsLeft = (then - Date.now()) / 1000;
    console.log(secondsLeft);
  }, 1000);
}
```

- 아래 콘솔!

<img width="300" alt="image" src="https://user-images.githubusercontent.com/82592845/163334817-441045e9-e1b2-49e2-9535-e8a790447f84.png">

- 소수점 없애기 위해 Math.round((then - Date.now()) / 1000); 로 묶는다.
- 이 것의 문제점은 멈추지않고 음수( - ) 로 계속 내려간다.

## 위의 해결 과정

```jsx
setInterval(() => {
  // then - now 는 now+seconds*1000 - now
  const secondsLeft = Math.round((then - Date.now()) / 1000);
  // check if we should stop it!!/
  // 멈추는 방법 1
  if (secondsLeft < 0) {
    return;
  }

  console.log(secondsLeft);
}, 1000);
```

- if(secondsLeft <= 0) { return; } 을하게 되면 실행은 하나 우리에게 보이는것이 없다.
  그러므로 밑의 방식을 시도한다.

```jsx
let countdown;  변수 선언

countdown = setInterval(() => {
    // then - now 는 now+seconds*1000 - now
    const secondsLeft = Math.round((then - Date.now()) / 1000);
    // check if we should stop it!!/
    // 멈추는 방법 1
    if(secondsLeft < 0) {
				clearInterval(countdown);
        return;
    }

    console.log(secondsLeft);
}, 1000);
```

<img width="381" alt="image" src="https://user-images.githubusercontent.com/82592845/163334778-46a4d0d9-2850-408e-bd94-0243ad1e93d6.png">

- 여기까지 하면 실행될때 바로 4, 3, 2, 1, 0 이 아닌 1초기다리고 3, 2, 1, 0 이렇게 실행된다.

## 해결위해 displayTimeLeft() 함수를 만든다.

- 위의 콘솔로그를 여기다 넣는다.

```jsx
function timer(seconds) {
  const now = Date.now();
  const then = now + seconds * 1000;

  // 여기에 함수가 실행되자마자 보여주는것을 추가해서 자연스럽게 시간이 뜨게한다.
  displayTimeLeft(seconds);

  countdown = setInterval(() => {
    const secondsLeft = Math.round((then - Date.now()) / 1000);

    if (secondsLeft < 0) {
      clearInterval(countdown);
      return;
    }
    // console을 다른 함수의 것으로 뜨게함.
    displayTimeLeft(secondsLeft);
  }, 1000);
}

function displayTimeLeft(seconds) {
  console.log(seconds);
}
```

<img width="526" alt="image" src="https://user-images.githubusercontent.com/82592845/163334849-0a11362c-7854-4303-8980-7014aedb843c.png">

## 이제 분&초 단위로 가보자!

```jsx
function displayTimeLeft(seconds) {
  const minutes = Math.floor(seconds / 60);
  const remainderSeconds = seconds % 60;
  console.log({ minutes });
}
```

<img width="641" alt="image" src="https://user-images.githubusercontent.com/82592845/163334879-904166d4-6821-47e8-8caa-94385fa7d479.png">

## 이제 화면에 뜨도록 해보자

```jsx
// html 요소 가져오기 const timerDisplay =
document.querySelector(".display__time-left");
function displayTimeLeft(seconds) {
  console.log(seconds);
  const minutes = Math.floor(seconds / 60);
  const remainderSeconds = seconds % 60;
  const display = `${minutes}:${remainderSeconds}`;
  timerDisplay.textContent = display;
}

timer(122);
```

<img width="339" alt="image" src="https://user-images.githubusercontent.com/82592845/163334902-309bd14e-64c3-4d3a-8ae5-d1ef8d544170.png">

- 이렇게 하면 초단위가 10 미만일때 01 02 03 04 05 이렇게가 아닌 1 2 3 4 5 이렇게 나온다.

## 해결 하는 방법은!!

```jsx
const display = `${minutes}:${
  remainderSeconds < 10 ? "0" : ""
}${reaminderSeconds}`;
```

- 이렇게 삼항연산자로 0 또는 아무것도 하지않는것을 추가해준다.

## 브라우저 타이틀과 타이머의 시간을 합체시키장!

```jsx
document.title = display;
```

---

## 이제 타이머가 끝난 후의 시간을 알아보자!

### 1. 어떤 방법으로 할지 한번 보자!

```jsx
function displayEndTime(timestamp) {
  const end = new Date(timestamp);
}
```

- 콘솔창에서 Date.now()를 하면 1632708232762 이런식으로 나오게 된다.
- 이것을 콘솔창에 new Date(1632708232762); 하게되면 보기좋게 변환해준다

<img width="434" alt="image" src="https://user-images.githubusercontent.com/82592845/163334924-a271ddc8-4eae-4f3b-be0b-5d7fc62789a6.png">

- 이런식으로 응용도 가능하다!!

<img width="336" alt="image" src="https://user-images.githubusercontent.com/82592845/163334943-36163549-6657-4115-b89b-6ff9e562ec6f.png">

- new Date 를 이용 , getHours, getMinutes를 이용하자 !

### 2. html 요소를 변수에 담고 textContent를 이용하자.

```jsx
const endTime = document.querySelector('.display__end-time');

function displayEndTime(timestamp) {
    const end = new Date(timestamp);
    const hour = end.getHours();
    const minutes = end.getMinutes();
    endTime.textContent = `Be Back At ${hour}:${minutes}`;
}
/// 이렇게 텍트트가 나올수 있게 한 후 timer함수에 가서

displayEndTime(then); 를 추가해준다.
```

- then은 now + seconds \* 1000; 이므로 타이머가 끝날시간이 된다.
- 여기까지 하게 되면 유럽식 시간 표시가 되는데 이것도 해결해보자!! ( ex⇒ 15: 14)

```jsx
const adjustedHour = hour > 12 ? hour - 12 : hour;
endTime.textContent = `Be Back At ${adjustedHour}:${
  minutes < 10 ? "0" : ""
}${minutes}`;
```

- 이렇게 하면 console 에 timer() 해서 실행이 잘된다.

## 버튼들들을 활성화 시켜보자!!

### 1.버튼 불러오기 , click이벤트로 startTimer() 바인딩

```jsx
const buttons = document.querySelectorAll("[data-time]");

function startTimer() {
  console.log(this.dataset.time);
}

buttons.forEach((button) => button.addEventListener("click", startTimer));
```

- console.log(this) 하면 눌린버튼이 나오고
- console.log(this.dataset.time) 하여 버튼을 누르면 이렇게 string of the number of minutes를 준다.

<img width="383" alt="image" src="https://user-images.githubusercontent.com/82592845/163334976-62293b0f-12ac-4ece-8286-9b68fbecadd8.png">
    
- 이제 이것을 이용하자!

### 2. 위를 이용!

```jsx
function startTimer() {
  const seconds = parseInt(this.dataset.time);
  timer(seconds);
}
```

- 이렇게 하면 처음누른 버튼은 작동한다. 하지만 다른 버튼을 누른경우 그 버튼을 실행했다가 바로 다시 전으로 돌아온다. 20sec ⇒ work 5 ⇒ 20sec
- 이것은 timer() 함수에 자기혼자 끝내는 기능이 없고 secondsLeft <= 0 일때만 끝나도록 되어있기 때문이다.
- 그러므로 timer()함수에

<aside>
💡 clearInterval(countdown); 를 추가하면 바로 해결이 된다.

</aside>

## 마지막으로 직접입력하는 부분을 해결해보자.

- 그전에 이렇게 불러올수도 있다는것을 알자!!

<img width="526" alt="image" src="https://user-images.githubusercontent.com/82592845/163335004-0bce90f6-f540-4fb2-b04f-3c3a597f4669.png">

- 이것을 이용해보자!

  ```jsx
  document.customForm.addEventListener("submit", function (e) {
    e.preventDefault();
    const mins = this.minutes.value;
    console.log(mins);
    timer(mins * 60);
    this.reset();
  });
  ```
