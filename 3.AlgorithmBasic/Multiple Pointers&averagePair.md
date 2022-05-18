# Multiple Pointers - average Pair

<img width="914" alt="image" src="https://user-images.githubusercontent.com/82592845/169041627-a45b58ee-1746-4392-b52a-d1398dcfcc74.png">

### 배열과 평균값이 매개변수로 들어간다.이때 배열의 요소들이 한쌍을 이루어 합을 구해 평균값을 계산했을때 매개변수로 주어진 평균값에 해당하는 값이 나온다면 true 나오지 않으면 false를 반환하라!

```jsx
function averagePair(arr, avg) {
  let start = 0;
  let end = arr.length - 1;
  while (start < end) {
    let avarage = (arr[start] + arr[end]) / 2;
    if (avarage === avg) return true;
    else if (avarage < avg) start++;
    else if (avarage > avg) end--;
  }
  return false;
}
```

- 배열의 시작 인덱스와 끝인덱스를 잡아 다중 포인터 방식으로 해결했다.
- 해당하는 한쌍의 요소의 avarage의 값을 avarage 에 계속 재 할당 되게 했고
- 주어진 평균값과 일치하면 true를 반환
- 계산된 평균값보다 주어진 평균값이 더 크면 시작 인덱스 ++
- 반대의 경우엔 끝 인덱스 -- 하는 방식으로 진행했다.
