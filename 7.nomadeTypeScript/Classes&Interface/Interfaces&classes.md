# Interfaces & Classes Combination

- type과 interface 둘다 오브젝트의 구조를 알려주는 작업을 수행할 수 있지만, 때에 따라 선호하게 되는 기능들이 있다.
- 추상 클래스는 , 다른 클래스가 가져야 할 property와 메소드를 명시할 수 있도록 도와준다.
- 복습겸 User 추상 클래스와 Player 클래스를 구현해보자!

```jsx
abstract class User {
    constructor(
        protected firstName:string,
        protected lastName: string,
    ) {}
    abstract sayHi(name:string):string
    abstract fullName():string
}

// new User()   불가능하다. 추상클래스의 인스턴스를 만들수 없기 때문

class Player extends User {
    fullName() {
        return `${this.firstName} ${this.lastName}`
    }
    sayHi(name:string) {
        return `Hello ${name}. My name is ${this.fullName()}`
    }
}
```

- 추상클래스를 만들었고, 추상클래스는 상속받는 다른 클래스가 가질 property와 메소드를 지정하도록 해준다.
- protected는 추상클래스로부터 상속받은 클래스들이 property에 접근하도록 해준다.
- 추상클래스의 문제점은, 자바스크립트에는 abstract의 개념이 없다는 것이다. 컴파일된 자바스크립트 코드를 보면 일반적인 클래스다.
- 우리는 다른 클래스들이 표준화된 모양( property와 메소드)를 갖도록 해주는 blueprint(청사진)을 만들기 위해 추상클래스를 사용한다.
- 우리는 User를 직접적으로 만드는것이 아니라 Player를 여러개 생성할 것이다.
- 하지만 추상클래스를 만든 코드는 자바스크립트 코드 결과에 존재한다.
- 이때 인터페이스를 사용해야할 때다.
  - 인터페이스는 가볍다, 그래서 컴파일하면 JS로 바뀌지 않고 사라진다.
  - 그럼, 인터페이스를 사용할때 클래스가 특정 형태를 따르도록 어떻게 강제할것인가?
  - 아래에서 알아보자! 추상클래스 → 인터페이스
  ```jsx
  interface User {          // 1번
      firstName:string,
      lastName:string,
      sayHi(name:string):string
      fullName():string
  }
  interface Human {
      health:number
  }

  class Player implements User, Human {
      constructor(
          public firstName:string,
          public lastName:string,
          public health:number
      ) {}
      fullName() {
          return `${this.firstName} ${this.lastName}`
      }
      sayHi(name:string) {
          return `Hello ${name}. My name is ${this.fullName()}`
      }
  }
  ```
  - 인터페이스는 constructor가 없고 추상 메소드도 없다.
  - 하지만 오브젝트나 클래스의 모양을 묘사하도록 해준다.
  - extends를 사용하면 자바스크립트로 변환된다. 자바스크립트는 클래스 뒤에 extends를 붙이는 문법을 사용한다. 이를 통해 클래스를 상속받을 수 있다.
  - 고로 implements라는 자바스크립트가 사용하지 않는 단어를 쓴다. 이를 사용하면 코드가 더 가벼워 진다. 또한 User인터페이스를 추적할 수 없는데, 인터페이스는 타입스크립트에서만 존재하며, 자바스크립트에서는 존재하지않지만 타입스크립트가 Player는 User 인터페이스를 상속해야한다고 알려준다. ⇒ Player가 나열된 property들을 가지고 있지 않다고 알려줌.
  - 인터페이스를 상속할 때에는 property를 private로 만들지 못한다. → 인터페이스를 만들 때 , property들이 public으로 되어있기 때문이다.
  → 이제 컴파일된 자바스크립트 코드에는 더 이상 추상클래스를 추가로 사용하지 않는다.
  → 인터페이스를 상속하는 것의 문제점중 하나는 private property 들을 사용하지 못한다는것과 추상클래스에선 constructor가 이 문제점을 해결해주도록 할 수 있었지만, 인터페이스를 사용하면 못한다.
  → 원하면 하나 이상의 인터페이스를 동시에 상속할 수도 있다.
  → 어댑터 패턴과 같은 디자인 패턴을 사용하여 팀으로 일할때에 인터페이스를 만들어두고 팀원이 원하는 각자의 방식으로 클래스를 상속하도록 하는것은 아주 좋은 방법이다.
  → 만약 모두 같은 인터페이스를 사용하면, 같은 property와 method를 가지게 되는것이다.
  ⇒ 다시 강조하자면, 1번은 클래스가 아니지만 클래스의 모양을 특정할 수 있게 해주는 간단한 방법이다. 즉, 오브젝트 모양 결정, 클래스 모양 특정이 가능하다.
  ```jsx
  function makeUser(user: User) {
  		return "hi"
  }

  makeUser({
  	firstName:"woo",
  	lastName:"joo",
  	fullName: () => "xx"
  	sayHi: (name) => "string"
  })
  ```
  ⇒ 다시 확인! 클래스를 타입으로 쓸 수 있고, 인터페이스도 타입으로 사용가능하다.
  - 볼 수 있듯 argument에 인터페이스를 사용하여 오브젝트의 모양을 지정해 줄 수도 있다.
  - 하지만 아래처럼 내가 인터페이스를 반환한다면, 타입을 리턴하는것처럼
    ```jsx
    function makeUser(user: User): User{
    		return {
    			firstName:"woo",
    			lastName:"joo",
    			fullName: () => "xx",
    			sayHi: (name) => "string"
    		}
    }

    makeUser({
    	firstName:"woo",
    	lastName:"joo",
    	fullName: () => "xx"
    	sayHi: (name) => "string"
    })

    // 다름을 보여주기위한 class 리턴
    function makeUser(user: User): User{
    		new User {

    		}
    }
    ```
  - new 다음에 클래스를 넣어줘야 하는 class리턴과는 다르다
  - 인터페이스를 리턴한다면 new User() 처럼 사용하지 않아도 된다.
  - 그냥 인터페이스의 내용만 넣어주면 된다.
