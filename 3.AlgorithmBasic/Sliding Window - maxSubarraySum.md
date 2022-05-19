# Sliding Window - maxSubarraySum

<img width="919" alt="image" src="https://user-images.githubusercontent.com/82592845/169234799-51fe3eeb-d189-48a0-ab7c-da1c643c903e.png">

## 문제

배열과 숫자가 주어진다. 주어진 숫자만큼 배열에서 연속되는 요소들의 총 합을 출력해라

```jsx
function maxSubarraySum(arr, num) {
  if (arr.length < num) return null;
  let sum = 0;
  for (let i = 0; i < num; i++) {
    sum += arr[i];
  }
  let finalSum = sum;
  for (let i = num; i < arr.length; i++) {
    finalSum += arr[i] - arr[i - num];
    sum = Math.max(sum, finalSum);
  }
  return sum;
}
```

- 미리 주어진 숫자보다 배열의 길이가 작으면 null을 리턴한다.
- 합을 담을 변수를 선언하고 처음 이어지는 숫자의 합을 저장하고, 임시로 값을 저장해서 비교할 변수에도 할당해준다.
- 그 후로 하나씩 더 해서 값을 재할당한 후에 Math.max를 이용하여 둘 중 큰값을 다시 할당하는 방식이다.
