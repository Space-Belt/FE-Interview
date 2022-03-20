1. html / css 이용해서 틀을 잡는다.
2. css 에 적용되지않은 playing 이라는 트렌지션을 작성한다.
3. 소리 재생 함수를 만든다
   1. audio, key라는 변수를 눌린 키값을 준다.
   2. audio 재생시간을 조절하고
   3. audio를 실행시킨다
   4. 마지막으로 playing (transition css) 추가
4. 눌렸을때 transition 생겼다가 사라지게 하는 함수를 만든다.
   1. a(매개변수)의 propertyName이 transform이 아니면 return 시키고
   2. this에 있는 playing 을 제거해준다.
5. keys 변수에 .key를 넣어주고
   1. forEach를 이용해서 removeTransition 함수를 넣어준다.
6. window.addEventListener 를 이용해서 키가 눌리면 playSound 함수가 재생되게 한다.

```jsx
function playSound(a) {
       const audio  =   document.querySelector(`audio[data-key="${a.keyCode}"]`);
       const key    =   document.querySelector(`.key[data-key="${a.keyCode}"]`);
       if(!audio) return;
       audio.currentTime=0;
       audio.play();
       key.classList.add('playing');
   }

매개변수 a 를 받는 playSound 함수를 만든다.

1. audio, key 변수에 querySelector (css 선택자)를 사용해 눌린 키 a의 keyCode 값을 넣는다.
(data-key 값과 눌린 a.keyCode 값이 같기 때문에 data-key로 연결된 key와 audio가 작동할수 있게끔 된다.)

2. if(!audio) return; 으로 audio 가 없으면 return시킨다. (다른키가 눌렸을때)

3. audio.currentTime = 0; 을 사용한다. 이 부분이 없으면 각 키의 소리가 시작하고 끝나기 전에는
다시 그 키를 누를수 없지만 currentTime 을 사용해서 연타가 가능하게 한다.

4. audio.play(); play()메소드를 사용해서 실행시킨다.

5. key.classList.add() class리스트에 'playing'을 추가해서 css .playing이 실행되게 한다.
```

### **querySelector**

- \*querySelector()\*\*는 특정 name,id,class를 제한하지 않고 css선택자를 사용하여 요소를 찾습니다.

같은 id 또는 class 일 경우 스크립트의 **최상단 요소**만 로직에 포함합니다.

`querySelector(#id) => id 값 id를 가진 요소를 찾습니다. querySelector(.class) => class 값 class를 가진 요소를 찾습니다.`

### **querySelectorAll**

querySelector와 사용 방법은 동일하며 선택자를 선택하여 배열과 비슷한 객체인 **nodeList**를 반환합니다. 반환객체가 nodeList이기에 **for문** 또는 **forEach문**을 사용합니다.

아래 코드와 같이 ","를 사용하면 여러 요소를 한번에 가져올 수 있습니다.

`querySelectorAll("#id,.class")`

## currentTime

**`currentTime`**속성은 초 현재 재생 시간을 지정합니다.

의 값을 변경하여 `currentTime` 새로운 시간으로 미디어를 찾습니다.

속성 세트 또는 (초) 오디오 / 비디오 재생의 현재 위치를 반환합니다.

이 속성이 설정되어있는 경우, 플레이어는 지정된 위치로 이동한다.

### HTMLMediaElement.play()

이 메서드는 미디어 재생을 시작하려고 시도합니다. 재생이 성공적으로 시작되었을 때 해결 된 것을 반환합니다

[https://velog.io/@rimu/자바스크립트-classList.add-remove-contains-toggle](https://velog.io/@rimu/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-classList.add-remove-contains-toggle)

### classList

classList를 이용하면 클래스를 조작하는 다양한 메서드들을 쓸 수 있다.

```
classList.add
```

: 클래스를 필요에 따라 삽입한다.

```
classList.remove
```

: 클래스를 필요에 따라 제거한다.

**_classList.contains( )_**

> classList.contains : 값이 존재하는지 체크한다. (true/false)

```
function handleClick() {
  const hasClass = title.classList.contains(CLICKED_CLASS);

  if (!hasClass) {
    title.classList.add(CLICKED_CLASS);
  } else {
    title.classList.remove(CLICKED_CLASS);
  }
}
```

**_classList.toggle( )_**

> classList.toggle(): 클래스값이 있는지 체크하고 없으면 더하고 있으면 제거한다.

```
function handleClick(){
 title.classList.toggle(CLICKED_CLASS);
}
```

```jsx
function removeTransition(a) {
       if(a.propertyName !== 'transform') return;
       this.classList.remove('playing');
   }

1. a 매개변수를 받는 removeTransition 이름의 함수를 만든다.

2. a.propertyName 이 transform 이 아닐경우 리턴시킴.

3. 눌린 키(a)의 클래스리스트에서 playing 클래스를 지워서 css를 사라지게 한다.

```

### propertyName ([https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Working_with_Objects](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Working_with_Objects))

자바스크립트의 객체에는 그와 연관된 프로퍼티가 있다. 프로퍼티는 객체에 붙은 변수(variable)라고 설명할 수 있겠다. 객체의 프로퍼티는 일반 자바스크립의 변수와 기본적으로 똑같은데, 다만 객체에 속해있다는 차이만 있을 뿐이다. 객체의 프로퍼티들이 객체의 특징을 규정한다.

```jsx
const keys = document.querySelectorAll(".key");
keys.forEach(key => key.addEventListener('transitionend', removeTransition));
window.addEventListener('keydown', playSound);

1. keys 변수에 querySelectorAll 을 사용해 div.key 클래스를 모두 넣는다.

2. forEach를 사용해서 key에 transitionend 트렌지션이 끝났을때 removeTransition 함수를 추가한다.

3. window.addEventListener('keydown', playSound);
	위 함수는
window.addEventListener('keydown', function(a) {

});
이 함수를 따로 하게 해준 함수이다.

윈도우 자체에서 키가 눌렸을때 playsound 함수를 실행시키도록 한다.

```

## **transitionend Event란?**

- transition이 **`완료된 이후`**에 발생하는 이벤트, transition 완료를 감지한다.
- transition과 함께 사용하는 함수
- **`addEventListener`**를 사용하여 이벤트 모니터링 가능

```jsx
window.addEventListener('keydown', function(e) {
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    const key   = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    if(!audio) return;
    audio.currentTime = 0;
    audio.play();
    key.classList.add('playing');

  });

이렇게 사용할 수 있는것을

function playSound(w) {
    const audio = document.querySelector(`audio[data-key="${w.keyCode}"]`);
    const key   = document.querySelector(`.key[data-key="${w.keyCode}"]`);
    if(!audio) return;
    audio.currentTime = 0;
    audio.play();
    key.classList.add('playing');
  }

window.addEventListener('keydown', playSound);

이렇게 사용가능하다.
```
