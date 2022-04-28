# Why TypeScript?

---

> 우리가 사용하는 브라우저들은 타입스크립트를 이해할 수 없다. 결국 자바스크립트로 변환해서 실행할 수 있다.

- 이렇게 하면 복잡해보이는데 왜 사용하는가??
- 아래의 코드로 알아보자!

```jsx
function add(num1, num2) {
  console.log(num1 + num2);
}

add(); // NaN
add(1); // NaN
add(1, 2); // 3
add(3, 4, 5); // 7
add("hello", "world"); // helloworld
```

- 보듯이 문자열로 전달해도 되고, 3,4,5 를 입력해도 5를 제외한 합을 반환해준다.
- 이렇게 1,2 를 매개변수로 제공한것 말고는 원하는답을 얻을수 없다.
- 즉 자바스크립트는 문제가 있지만 어떤 문제가 있는지 친절하게 알려주지 않고 그냥 진행한다.

```jsx
function showItems(arr) {
  arr.forEach((item) => {
    console.log(item);
  });
}

showItems([1, 2, 3]);
showItems(1, 2, 3);
```

- 여기선 배열을 전달하면 잘 작동하지만 숫자들을 전달하면 타입에러가 발생한다. (1,2,3)에는 forEach라는 메소드가 없기 때문인다.
- 즉 자바스크립트(동적언어)는 실행되는 시점, 즉 런타임에 타입이 결정되고 오류가 있으면 그때 발견된다.
- 반면 자바,TypeScript 같은 정적언어는 컴파일시에 타입이 결정 되고, 오류를 발견하게 된다. 그래서 코드작성 시간은 길어질수 있지만 제대로 계획하고 작성하면 안정적이고 빠르게 작업을 진행 할 수 있다.

### 위의 코드들을 타입스크립트문법으로 고쳐보자 ! ( [typeScript PlayGround](https://www.youtube.com/watch?v=5oGAkQsGWkc&list=PLZKTXPmaJk8KhKQ_BILr1JKCJbR0EGlx0) )

- 첫번째 코드
  - any타입은 아무타입이 다들어와도 되는것이다.
  - 아래와 같이 어떤 오류가 있는지 알 수 있다.

<img alt="image" src="https://user-images.githubusercontent.com/82592845/165673681-dad9f0a1-20f5-49f0-81a4-90a5c226c5a3.png">

<img alt="image" src="https://user-images.githubusercontent.com/82592845/165673883-9f620d88-268b-4708-98fc-f5ba46c9b3e2.png">

```jsx
function add(num1: number, num2: number) {
  console.log(num1 + num2);
}

add(); // err
add(1); // err
add(1, 2); // 3
add(3, 4, 5); // err
add("hello", "world"); // err
```

- 이렇게 타입을 지정해 준다.
- 에러가 있는곳을 표시 해준다!

### 두번째 코드

```jsx
function showItems(arr: number[]) {
  arr.forEach((item) => {
    console.log(item);
  });
}

showItems([1, 2, 3]); /// 1  2  3
showItems(1, 2, 3); // arr
```

- 숫자 배열이라는 타입을 지정해준것이다.
- arr:number 만 하게 되면 forEach 메소드가 없기 때문에 오류가 발생한다.
