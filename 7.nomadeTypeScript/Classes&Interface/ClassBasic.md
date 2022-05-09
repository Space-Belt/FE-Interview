# Classes

---

> 타입스크립트로 객체지향 프로그래밍을 알아볼 것이다

- 타입스크립트에선 private method, public property, abstract class, polymorthism, generic 같은 모든것들을 사용 가능하다.
- 타입스크립트로 클래스를 만드는 법을 알아보자!

```jsx
class Player {
    constructor(
        private firstName:string,
        private lastName:string,
        public nickname:string
    ) {}
}

const woo = new Player("woo", "joo", "우주");

woo.nickname;
```

- 보통 자바스크립트에서는 Constructor 함수를 만들고 그 안에 `this.firstName = firstName` 혹은 `this.lastName = lastName` 같은 코드를 넣는다.
  - 하지만 타입스크립트에서는 그렇게 하지 않아도 된다.
  - 파라미터를 써주기만 하면, 알아서 Contructor 함수를 만들어준다.
  - 위 코드에서 보듯이 private 혹은 public property를 만들 수 있다.
  - 아래에서 보면 자바스크립트에서는 private가 보이지 않는다. 자바스크립트로 컴파일 되면서 안보인다.
  - <img width="320" alt="image" src="[https://user-images.githubusercontent.com/82592845/167245997-041d1d60-45ad-41a0-9cc9-a8a90d50a4f5.png](https://user-images.githubusercontent.com/82592845/167245997-041d1d60-45ad-41a0-9cc9-a8a90d50a4f5.png)">
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/54259aec-df99-4ca6-b251-fff6ce9484e5/Untitled.png)
- woo.firstName 은 private로 설정했기 때문에 오류가 발생한다.
- 타입스크립트와 객체지향 프로그래밍을 좋은점은 추상 클래스다. 아래에서 알아보자!

```jsx
// 1
abstract class User {
    constructor(
        private firstName:string,
        private lastName:string,
        public nickname:string
    ) {}
}

class Player extends User {

}

const joo = new User("woo", "joo", "우주"); // 1 번 오류

const woo = new Player("woo", "joo", "우주"); // 가능

// 2. 추상 클래스 안의 메소드랑 abstract method 알아보자!
abstract class User {
    constructor(
        private firstName:string,
        private lastName:string,
        public nickname:string
    ) {}
    getFullName() {
        return `${this.firstName} ${this.lastName}`  //  (여기여기)
    }
}

class Player extends User {

}

const woo = new Player("woo", "joo", "우주");

woo.nickname;
woo.getFullName();
```

- 추상 클래스는 다른 클래스가 상속받을 수 있는 클래스다.
- 하지만 직접 새로운 인스턴스를 만들 수는 없다. → 1 번 오류
  - 즉 추상 클래스는 오직 다른곳에서 상속만 가능한 클래스다.
- 또한 2 번에서 method에 private를 줄 수 있다. 즉 메소드에도 가능하다는것이다.
- 우리가 직접 추상 메소드를 만들 수 있는데, 만드려면 메소드를 클래스 안에서 구현하지 않으면 된다. → (여기여기) 부분을 작성하지 않으면 된다!!!! 이 부분이 메소드의 implementation(구현)이다.
- 메소드는 클래스 안에 존재하는 함수!

⇒ 추상 클래스 안에서는 추상 메소드를 만들 수 있다. 하지만 메소드를 구현해서는 안되고 대신에 메소드의 call siganture 만 적어둬야 한다.

⇒ 예들들어 User 추상클래스 안에 getNickName 추상메소드가 있다고 가정하자.(말 안됨)

```jsx
abstract class User {
    constructor(
        private firstName:string,
        private lastName:string,
        protected nickname:string
    ) {}
		abstract getNickName():void
    getFullName() {
        return `${this.firstName} ${this.lastName}`  //  (여기여기)
    }
}

class Player extends User {  // 오류
	// 오류 해결
		getNickName() {
			console.log(this.nickname);
		}

}
```

- `abstract getNickName():void` 이 부분이 callsignature 부분이다.
- 이렇게 하면 `class Player extends User` 에서 getNickName을 구현해야 한다고 알려준다.
- 그렇다면 추상메소드는 무엇일까? → 추상 클래스를 상속받는 모든 것들을 구현을 해야하는 메소드를 의미한다.
- 오류 해결 부분에서 nickname을 제외한 것들에는 접근 할 수 없다. → private 는 개인적인 것이고, User클래스의 인스턴스나 메소드에서는 접근 가능하나, 클래스가 추상 클래스라 인스턴스 화 할 수 없기 때문에 오류가 난것이다. 즉, 외부에서는 보호되지만 다른 자식 클래스에서는 사용되기를 원한다면 protected를 사용하자!
