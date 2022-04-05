> <h2>각 배열에 겹치지 않는 몇개의 숫자가 있는지</h2>

<img width="437" alt="image" src="https://user-images.githubusercontent.com/82592845/161722997-25276211-b1e4-42eb-a488-d6296fed301e.png">

<br />

> <h3>첫번째 풀이</h3>

```jsx
function countUniqueValues(arr) {
  const stst = arr.toString().replace(",", "");
  if (arr.length === 0) {
    return 0;
  }
  const obj = {};
  for (let a of stst) {
    obj[a] = (obj[a] || 0) + 1;
  }

  return Object.keys(obj).length;
  // console.log(obj.length);
}

countUniqueValues([1, 1, 1, 1, 1, 2]);
```

- 배열을 스트링으로 만들기도 하고 , 제거 하고 Object.method를 이용했다.

<br />

> <h3>다중포인터 챕터에 맞는 풀이</h3>

```jsx
function countUniqueValues(arr) {
  var i = 0;
  for (var j = 1; j < arr.length; j++) {
    // i 배열과 j배열이 같지 않은지를 확인하여 유효하지 않은 구문을 감싸서 조건문으로
    if (arr[i] !== arr[j]) {
      i++;
      arr[i] = arr[j];
    }
  }
  return i + 1;
}

countUniqueValues([1, 1, 1, 1, 1, 2]);
```

- 아래와 같은 로직이다.

```jsx
i
[1,1,1,2,3,3,4,4,5,6]
   j

 i
[1,1,1,2,3,3,4,4,5,6]
     j

 i
[1,1,1,2,3,3,4,4,5,6]
       j

   i
[1,2,1,2,3,3,4,4,5,6]
       j

   i
[1,2,1,2,3,3,4,4,5,6]
         j

     i
[1,2,3,2,3,3,4,4,5,6]
         j

     i
[1,2,3,2,3,3,4,4,5,6]
           j

     i
[1,2,3,2,3,3,4,4,5,6]
             j

       i
[1,2,3,4,3,3,4,4,5,6]
             j

       i
[1,2,3,4,3,3,4,4,5,6]
               j

       i
[1,2,3,4,3,3,4,4,5,6]
                 j

         i
[1,2,3,4,5,3,4,4,5,6]
                 j

         i
[1,2,3,4,5,3,4,4,5,6]
                   j

           i
[1,2,3,4,5,6,4,4,5,6]
                   j

i 위치의 indexNumber + 1
```
