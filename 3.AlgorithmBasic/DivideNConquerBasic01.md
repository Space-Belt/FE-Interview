# 분할 정복 알고리즘 패턴

<img alt="image" src="https://user-images.githubusercontent.com/82592845/162181158-569b367e-ea50-4cf8-87b8-bc03b15087b3.png">

실제 있는 패턴 이름이다!

지금까지 해왔던 패던들과는 좀 다르다. (복잡하다)

> <h2>개념</h2>

- 주로 배열이나 문자열 같은 큰 규모의 데이터셋을 처리한다.
- 연결 리스트나 트리를 처리할 수도 있다.
- 일단은 배열하나로 알아보자!
- 값을 찾기 위해 배열의 왼쪽에서 시작해 오른쪽 끝까지 이동하는 것 보다 배열을 작은 조각으로 세분하여 각 조각들을 어디로 이동 시킬지 결정하는 작업부터 해보자! 즉, 큰 데이터 → 작은 데이터 조각 ( 문제에 따라 잘 사용하자. 탐색 알고리즘에서 알아본 이진 탐색 혹은 그냥 탐색 알고리즘에서 좋다)

    <img width="523" alt="image" src="https://user-images.githubusercontent.com/82592845/162181970-e0c59814-f21c-4270-afdb-b639ad646212.png">
    
    - 정렬된 배열을 취한다.
    - 정수로 이루어진 배열과 정수를 받는데 정수의 값에 해당하는 값을 인덱스를한다.

    <br />

  > <h3>선형탐색(Linear Search) 풀이<h3>

  ```jsx
  function search(arr, val) {
    for (let i = 0; i < arr.length; i++) {
      if (arr[i] === val) {
        return i;
      }
    }
    return -1;
  }
  ```

  - O(n)의 시간 복잡도

    <br />

  > <h3>이진 탐색 풀이  (분할 정복 알고리즘 Binary Search)</h3>

  ```jsx
  function search(arr, val) {
    let min = 0;
    let max = array.length - 1;

    while (min <= max) {
      let middle = Math.floor((min + max) / 2);
      let currentElemnet = array[middle];

      if (array[middle] < val) {
        min = middle + 1;
      } else if (array[middle] > val) {
        max = middle - 1;
      } else {
        return middle;
      }
    }
    return -1;
  }

  // [1,2,3,4,5,6,8,9,12,15,16,20] , 15
  // 15를 찾을때까지 루프를 진행하는 방법이있다.(선형탐색)
  // 또는 배열을 나누는 이진 탐색을 사용할 수 있다.
  ```

  - Log(n) 시간복잡도를 가지고 있다. ( Binary Search )
  - 분할 정복 알고리즘이다.
  - 정렬이 되어있으니 중간 지점을 선택한다.
  - 중간을 기준으로 찾는 값이 기준보다 큰지 작은지를 확인할 수있다. 그러면 기준값보다 작거나 큰값을 제외 할 수 있다.
  - 그 다음 정복을 해야한다.(conquer)
  - 계속해서 중간지점을 찾아서 제외할 영역을 제외하여 찾아낸다.
  - 선형탐색보다 훨씬 빠르다. 하지만 코드가 좀 더 복잡하다.

⇒ 분할과 정복은 아주 보편적으로 많이 쓰인다.
