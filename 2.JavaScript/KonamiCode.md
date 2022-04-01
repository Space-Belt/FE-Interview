> 목표

> 키워드를 지정 후 그 키워드가 입력되었을때 화면에 띄워지는 그림과 몇번 입력되었는지 확인 시켜주는 문구를 만들어보자!

<aside>
💡 const pressed = [];

</aside>

### 1. 눌려진 키가 들어갈 배열을 생성한다.

<aside>
💡 const secretCode= 'woojoo';

</aside>

### 2. 시크릿키코드를 지정해준다.

```html
window.addEventListener('keyup', (e) => {

	console.log(e.key);
	// 배열에 담는다.
	pressed.push(e.key);
	// splice ?  메서드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.
	// -secretCode.length -1 뒤에서내려오게 , pressed 배열의 길이에서 woojoo의 길이를 뺌 )
	// splice()의 첫 번째 파라미터가 음수일 경우, 잘라낼 요소를 뒤에서부터 카운팅
	// 배열의 길이가 왜 6개로 고정되는지 잘 모르겠다.
	// -secretCode.length - 1    6개가 되면 뒤에서부터 줄여나간다(?)
	pressed.splice(-secretCode.length - 1, pressed.length - secretCode.length);
	if(pressed.join('').includes(secretCode)) { // 합친게 woojoo가 되면 실행
	    console.log('DING DING!');
	    cornify_add(); // <script> 부분에서 불러온 유니콘 사진을 화면에 띄운다.
	}
	console.log(pressed);
});
```

### 3. woojoo가 입력되면 짤리게 만들어 준다.

- splice = > splice(시작, 자를개수)
- join('')로 문자열로 만든후 includes함수로 secretCode를 포함하고 있는지 확인 후 실행)
