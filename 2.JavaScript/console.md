## Console 공부 !

## 목표

> console 에서 지원하는 기능들 알아보기

## 1. Regular

<aside>
💡 console.log('hello');

</aside>

- 그냥 hello 출력

## 2. Interpolated

<aside>
💡 console.log('Hello I am a %s string!', '😄');

</aside>

- %s 대신 이모티콘

## 3. styled

<aside>
💡 console.log('%c I am some great text', 'font-size:50px; background:red; text-shadow: 10px 10px 0 blue');

</aside>

- %c 스타일 적용 css

## 4. warning

<aside>
💡 console.warn('OH NOOO');

</aside>

<img width="640" alt="image" src="https://user-images.githubusercontent.com/82592845/161387054-b0dc5f52-24b5-4c7e-bf48-b4a99a9bac88.png">

## 5. error

<aside>
💡 console.error('Shit!');

</aside>

<img width="641" alt="image" src="https://user-images.githubusercontent.com/82592845/161387084-60f93396-4495-4e7f-b9c1-0b1b25927684.png">

## 6. Info

<aside>
💡 [console.info](http://console.info/)('Crocodiles eat 3-4 people per year');

</aside>

- log 랑 뭐가 다른지 잘 모르겠다. 정보를 표시

## 7. assert 예약어

<aside>
💡 console.assert(p.classList.contains('ouch'), 'That is wrong!');

</aside>

여기서는 p.classList.contains('ouch')의 true, false

- 주어진 가정이 거짓인 경우 콘솔에 오류 메시지를 출력합니다. 참인 경우, 아무것도 하지 않습니다.
    <img width="597" alt="image" src="https://user-images.githubusercontent.com/82592845/161387128-9bdb97ef-e695-4917-af93-54207a160d89.png">
- [https://www.notion.so/09-Dev-tools-70e566d5c295432bb94f305a167a8ff1#850e3799bd0b455bafb9c240beea6fbc](https://www.notion.so/09-Dev-tools-70e566d5c295432bb94f305a167a8ff1)

## 8. clearing

<aside>
💡 console.clear();

</aside>

- 콘솔창 청소 ( )

## 9. console.log(p)

- p 태그 전체를 출력한다.
- <p onclick="makeGreen()">×BREAK×DOWN×</p>

## 10. console.dir(p)

- JavaScript 개체의 속성에 대한 대화형 목록을 표시합니다. 출력은 자식 개체의 내용을 볼 수 있는 펼침 삼각형이 있는 계층적 목록으로 표시된다.
    <img width="601" alt="image" src="https://user-images.githubusercontent.com/82592845/161387154-9279b32f-c22e-4954-8b8a-a10cf440f0d4.png">


## 11. Grouping

```html
// Grouping together dogs.forEach(dog => { // console.group(`${dog.name}`); //
이거로 하면 펼쳐져서 나오고 // 이거로하면 내가 펼쳐야한다.
console.groupCollapsed(`${dog.name}`); console.log(`This is ${dog.name}`);
console.log(`${dog.name} is ${dog.age} years old`); console.log(`${dog.name} is
${dog.age * 7} dog years old`); console.groupEnd(`${dog.name}`); });
```

<img width="525" alt="image" src="https://user-images.githubusercontent.com/82592845/161387177-75ec5ac6-0d13-4bf2-91f4-79b5e70d192e.png">

## 12. Count

<aside>
💡 console.count('wes');

</aside>

<img width="635" alt="image" src="https://user-images.githubusercontent.com/82592845/161387200-a63e607f-6e75-427a-9e09-eb2c28ae44f0.png">

- 몇번 했는지 세준다.

## 13. timing (얼마나 걸렸는지 시간)

```html
console.time('fetching data'); fetch('https://api.github.com/users/wesbos')
.then(data => data.json()) .then(data => { console.timeEnd('fetching data');
console.log(data); });
```

먼저 fetching data 라고 명시를 해주고, 6번 프로젝트 게시물에 있는 fetch 함수를 사용합니다. 해당 [https://api.github.com/users/wesbos](https://api.github.com/users/wesbos) 사이트에 접속하여, data 를 .json() 으로 파싱하여, 추출한 데이터마다 timeEnd 함수를 사용합니다. 이때 인자는 맨 앞줄에 명시한 것과 같게 해야 가능합니다!
