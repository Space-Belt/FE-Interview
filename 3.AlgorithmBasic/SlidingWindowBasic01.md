# 기준점 간 이동 배열 패턴

<img alt="image" src="https://user-images.githubusercontent.com/82592845/162179341-2964efac-5a01-457d-b2c6-d181a854c4f7.png">

배열이나 문자열과 같은 일련의 데이터를 입력하거나 특정 방식으로 연속적인 해당 데이터의 하위 집합을 찾는 경우에 유용하다.

`“hellothere”` 이라는 문자열이 있다고 생각해보자. 이때 가장 긴 시퀀스의 고유 문자를 찾는 함수를 작성하라고 하면 `hel` 이 처음으로 제일 긴 고유 문자다. `ell` 은 안된다. 차례대로 보면 `lother` 이 가장 긴 고유 문자의 배열이라는 것을 알 수있다.

이 예시는 슬라이딩 윈도우 접근법으로 해결 가능하다.

다른 예시를 한번 알아보자 !

<img alt="image" src="https://user-images.githubusercontent.com/82592845/162179372-14aa6f44-db28-446d-a4d7-60bda3d05ed4.png">
배열과 숫자 하나를 전달하는데 처음 것은 서로 마주한 두 숫자의 합중 제일 큰 값을 출력하는 예시이다.

이 문제를 해결하는법!

창문을 하나 만들어야 한다. 이 창문은 단일 변수, 하위 배열, 또는 필요한 경우 다른 문자열도 될 수 있다. 조건에 따라 창문을 이동시키고, 시작 위치에서 시작하면 보통 왼쪽에서 오른쪽으로 이동한다. 오른쪽에서 왼쪽으로 이동도 가능하고 가운데에서 시작도 가능하다. 하지만 보통 창문을 왼쪽, 즉 요소의 시작위치, 또는 배열이나 문자열의 시작위치에서 끝나는 위치로 이동한다. 때에 따라서 새 창문을 만들기도 한다. 이 방법은 규모가 큰 데이터셋에서 데이터의 하위 집합을 추적하는 이런 문제에 있어서 유용하다.

> <h3>해결 해보자</h3>

```jsx
function maxSubarraySum(arr, num) {
  if (num > arr.length) {
    return null;
  }
  var max = -Infinity;
  for (let i = 0; i < arr.length - num + 1; i++) {
    temp = 0;
    for (let j = 0; j < num; j++) {
      temp += arr[i + j];
    }
    if (temp > max) {
      max = temp;
    }
  }
  return max;
}

maxSubarraySum([4, 4, 4, 4, 4, 15, 5, 1], 3);
// 24
```

1. 우선 주어지는 숫자가 배열의 길이보다 길면 null을 리턴해준다.
2. max를 선언하고 -Infinity에서 시작되도록 한다. 배열이 모두 음수로 구성 되어있으면 가장 큰 합은 음수이기 때문이다.
3. for문이 어디서 끝날지 num을 과 배열의 length로 정해준다.
4. 합계를 더해줄 temp를 선언하여 max와 값을 비교한다.
5. 2중 for문으로 num 만큼의 길이 동안만 값을 더하게 한다.
6. 값을 더하여 temp에 저장하고 max보다 크다면 max값을 변경하여 최대값을 찾는다.

⇒ 2중 for문 좋지 않다! (빅오표기법) O(n\*\*n)이다.

---

> <h3>개선된 해결법 (슬라이딩 윈도우 방식 적용)</h3>

- 두개의 루프가 있지만 배열 전체에 루프는 한번만 적용된다.

```jsx
function masxSubarraySum(arr, num) {
  let maxSum = 0;
  let tempSum = 0;
  if (arr.length < num) return null;
  for (let i = 0; i < num; i++) {
    maxSum += arr[i];
  }
  tempSum = maxSum;
  for (let i = num; i < arr.length; i++) {
    tempSum = tempSum - arr[i - num] + arr[i];
    maxSum = Math.max(maxSum, tempSum);
  }
  return maxSum;
}
// [1, 2, 3, 4, 5, 1, 2, 3, 4, 5],3
// 1~3 더하고 2~4 더하고 하는것이 아닌
// 1,2,3 더한것에서 1 빼고 4를 더해주는 로직
// 6 - 1 + 4 = 9
// 9 - 2 + 5 = 12
// 12 - 3 + 1 = 10
// 10 - 4 + 2 = 8
// 8 - 5 + 3 = 6
// 6 - 1 + 4 = 9
// 9 - 2 + 5 = 12
// 12
```

1. 우선 주어지는 숫자가 배열의 길이보다 길면 null을 리턴해준다.
2. 첫번째 합을 maxSum에 저장해준다.
3. tempSum에 maxSum의 값을 할당 해주고 마지막엔 maxSum을 리턴한다.
4. 진행하는동안 Math.max(a,b) 를 이용하여 큰값을 maxSum에 할당해준다.

⇒ 이렇게 중첩루프를 사용하지 않고 간단하게 진행된다.
