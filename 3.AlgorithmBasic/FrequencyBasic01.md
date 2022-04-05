# <b>빈도순환패턴<b/>

### 이 패턴은 보통 자바스크립트의 객체를 사용해서 다양한 값과 빈도를 수집하는 것이다.

### 여러개의 데이터와 입력값이 있고 그것들이 비슷한 값을 가졌는지 비교하거나 그것들이 서로의 아나그램(?)인지 확인해야 하거나, 한 값이 다른 값에 들어 있는지 알아내야 하거나, 언제든지 둘 이상의 입력값데이터들과 그 빈도를 비교해야하는 문제나 알고리즘에 대한 것을 다룬다며 아주 도움이 될것이다.

### 예시 !

- 이걸 좋은 접근법이자 유용한 패턴으로 만들어주는 것은 보통 이 답안이 빅오(n)의 시간을 가진다는 점인데, 이보다 더 쉽다고 여겨지는 답안들
- 예를 들면 중첩 루프는 n제곱의 시간을 가진다.

<img width="533" alt="image" src="https://user-images.githubusercontent.com/82592845/161532996-219439e1-0ae3-4ce5-bbeb-6d3d0594e1a7.png">

### 문제 : 두개의 배열을 입력하는 Same이라는 이름의 함수를 작성해라. 함수는 배열에 있는 모든 값에 대해 두번째 배열에 제곱을 한 값이 있다면 참을 출력해야 한다. 즉 첫번째 배열에는 많은 값들이 있고, 두번째 배열에는 똑같은 값들에 제곱을 한 값들이 있으면 된다. 하지만 순서는 상관 없다.(순서는 섞여도 된다) 그냥 같은 상태에서 제곱만 취하면 된다. ( 값의 출현 빈도만 같으면 된다 )

```jsx
// 중첩 루프를 사용한 순진한 답안
// 사실 루프를 하나만 썼지만 그 자체로 루프인 인덱스(indexOf)를 사용하고 있으니
// 중첩 루프라 볼 수 있다.
// 사실 indexof를 사용해서 해결 방법을 찾는 대신 전체 배열을 손수 돌면서 확인해도 되는데
// 물론 그 자체가 indexOf가 하는 일이긴 하다.
// 하나의 루프가 있고 또 다른 for 루프가 그 안에 있어서 이 녀석들에게 배열을 돌면서
// 제곱을 찾도록 할 수 있게 되는것이다.
// 이제 숫자를 보고 배열을 전체 적으로 도는데 여전히 splice는 해야한다.
// 같은 값을 이미 마주쳤는지 여부를 기억할 방법이 필요하기 때문이다.
// 이거는 순진한 접근법으로 bigO(n) 값을 가진다. (이차함수 관계)
// indexOf를 반복적으로 배열 전체에 사용하면 중첩 루프로 인해서 하루종일 계산을 할 수도 있다.
// n이 우리 배열들의 길이에 따라 늘어나면 이 값도 비례해서 커지고, 결국 제곱의 비율로 중첩된
// 루프가 된다.  n이 우리 배열들의 길이에 따라 늘어나면 이 값도 비례해서 커질 것이고, 걀국
// 제곱의 비율로 중첩된 루프가 될 것이다. 그러니 중첩루프는 피해야한다.
function same(arr1, arr2) {
  // 먼저 할 것은 아주 작은 경계조건을 넣어보는 것이다.
  // 실제로는 경계조건도 아니지만, 두 배열의 길이가 다른것을 바로 알 수 있기때문이다.
  if (arr1.length !== arr2.length) {
    return false;
  }
  for (let i = 0; i < arr1.length; i++) {
    let correctIndex = arr2.indexOf(arr1[i] ** 2);
    if (correctIndex === -1) {
      return false;
    }
    arr2.splice(correctIndex, 1);
  }
  return true;
}
```

### Frequency Counter 를 이용한 답

```jsx
// 이제 빈도 카운터 패턴으로 해보자
// 첫번쨰 배열에 루프를 걸어 각 값을 확인하고 다른 루프로 두번째 배열을 거는 대신
// 한번에 하나씩 두 배열에 각각 루프를 걸 수 있다.
// 루프 두개가 중첩된 두개의 루프보다 훨씬 더 좋다!
// 두 개의 객체를 사용해서 각각을 양쪽 배열의 깂의 빈도를 세는데 배치한다.
// 그러면 마지막에 두개의 객체가 남는다.
// 객체 대신에 데이터에 접근하는 것은 아주 빠르다.
// 빈도 카운터를 사용하는 방법은 기본적으로 객체를 사용하는 것이다.
// 객체를 사용해서 프로필을 만든다고 하면 배열이나 스트링의 요소
// 즉 배열과 스트링 같은 직선 구조를 가지고 작업을 한다는 말이다.
// 그러면 다른 배열이나 스트링을 가지고 만든 다른 객체와 이것을 빠르게 비교할 수있다.
// 두개의 배열을 객체로 쪼개고, 그 안에 있는것을 분류하고, 두객체를 비교했다.
function same2(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }
  let frequencyCounter1 = {};
  let frequencyCounter2 = {};
  // 객체에 각 값이 해당 배열에서 몇 번 등장하는 지를 종합한 것이다.
  for (let val of arr1) {
    frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1;
  }
  for (let val of arr2) {
    frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1;
  }
  for (let key in frequencyCounter1) {
    if (!(key ** 2 in frequencyCounter2)) {
      return false;
    }

    if (frequencyCounter2[key ** 2] !== frequencyCounter1[key]) {
      return false;
    }
  }
  return true;
}
```

<img width="465" alt="image" src="https://user-images.githubusercontent.com/82592845/161533078-10f45e75-3f57-4970-9772-40721da66091.png">

### for ...of 배열의 value 순환

### for ...in 객체의 property 순환
