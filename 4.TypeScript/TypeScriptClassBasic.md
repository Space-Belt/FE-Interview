# TypeScript Class 선언

- es class → 코딩앙마 javascript #15 class 참고

```jsx
// 1번 이렇게 하면 에러
class Car {
  constructor(color: string) {
    this.color = color; // 에러발생 마우스 올리면 color가 Car 클래스에 없다고 나온다.
  }
  start() {
    console.log("start");
  }
}

// 2번 정상 작동
class Car {
  color: string;
  constructor(color: string) {
    this.color = color;
  }
  start() {
    console.log("start");
  }
}

const bmw = new Car("red");
```

- 타입스크립트에서 클래스를 작성할때 멤버변수를 미리 선언해주어야 하기에 2번처럼 해줘야 작동한다.
- 멤버 변수를 미리 선언하지 않고 사용하는 방법도 존재한다.
  - 접근 제한자나, readonly 키워드를 이용하는 방법이다.
  ```jsx
  //접근 제한자
  class Car {
  	constructor(public color: string) {
  		this.color = color;
  	}
  	start() {
  		console.log("start");
  	}
  }

  class Car {
  	constructor(readonly color: string) {
  		this.color = color;
  	}
  	start() {
  		console.log("start");
  	}
  }
  ```

### public, readonly 키워드에 대해 알아보자!

```jsx
class Car {
	name: string = "car";
	color: string;
	constructor(color: string) {
		this.color = color;
	}
	start() {
		console.log("start");
	}
}

clsass Bmw extends Car {
	constructor(color: string){
		super(color);
	}
	showName() {
		console.log(super.name);
	}
}

const z4 = new Bmw("black");

```

- es6 의 클래스는 다른 객체지향언어들처럼 접근제한자를 지원하지 않았다. 하지만 타입스크립트에서는 지원한다!
- 접근제한자에는 public, private, protected 가 존재한다.
  - public → 자식클래스나 클래스 인스턴스에서 접근이 가능하다. 아무것도 표기하지 않고 사용하면 모두 public이다.
    - 위를 보면 car클래스가 있고 car를 상속받은 bmw가 있다. bmw에 constructor가 있고 super를 호출하고 있다. 여기서 super를 호출하지 않으면 오류가 발생한다.
    - bmw에는 showName()이라는 메소드가 있고 멤버변수 name을 보여준다. 즉 car의 이름이 public이기 때문에 이렇게 자식클래스 내부에서 접근해도 사용이 가능하다.
    - 명시적으로 class car {} 에서 public을 해줘도 똑같다.
    ```jsx
    class Car {
    	public name: string = "car";  -> private name: string = "car";
    	color: string;
    	constructor(color: string) {
    		this.color = color;
    	}
    	start() {
    		console.log("start");
    	}
    }
    ```
    - 하지만 만약 private로 바꾸게 되면 외부에서 사용할 수 없다. (car 클래스 내부에서만 사용 가능) private 는 #name: string = “car” 이렇게 사용해도 같다.
      ```jsx
      class Car {
      	#name: string = "car";
      	color: string;
      	constructor(color: string) {
      		this.color = color;
      	}
      	start() {
      		console.log("start");
      		console.log(this.#name);
      	}
      }

      class Bmw extends Car {
      	constructor(color: string) {
      		super(color);
      	}
      	showName() {
      		// 자식 클래스는 클래스 인스턴스로 접근하는 부분은 에러 발생!
      		console.log(super.#name);
      	}
      }

      const i5 = new Bmw("black");
      ```
      - 역시나 자식클래스에서는 오류가 발생 !
      - #을 붙여줘야한다. #과 private의 기능차이는 없다.
  ### protected에 대해서도 알아보자!
  - protected로 바꾸면 에러가 사라진다.
  - 접근제한자 protected도 자식클래스에서 접근이 가능하다.
  - 그럼 public과는 무슨 차이일까?
    - protected로 선언 후 `console.log(i5.name);` 을 해보면 오류가 발생한다.
    - 하지만 public으로 선언하면 클래스 인스턴스를 통해 접근이 가능하다 ( `[i5.name](http://i5.name)` )
    - protected는 자식클래스 내부에서는 참조가능하나, 클래스 인스턴스로는 존재할수 없다.
    ⇒ 퍼블릭은 자식 클래스나 클래스 인스턴스 모두 접근이 가능하다.
    ⇒ 프로택티드는 자식 클래스 내부에서만 접근이 가능하다.
  - public으로 선언했다면 `i5.name=”aaa5”` 이런식으로 i5의 내용을 변경할 수 있다.
  - name을 수정 못하게 하려면 readonly 키워드를 사용하면 된다. `readonly name: string = "car";` ( car class 내부 )
    - 만약 name을 바꾸고 싶다면 컨스트럭터 내부에서 그 작업을 할 수 있게 해줘야 한다.
    ```jsx
    class Car {
    	readonly name: string = "car";
    	color: string;
    	constructor(color: string, name) {
    		this.color = color;
    		this.name = name;
    	}
    	start() {
    		console.log("start");
    		console.log(this.name);
    	}
    }

    class Benz extends Car {
    	constructor(color: string, name) {
    		super(color, name);
    	}
    	showName() {
    		console.log(super.name);
    	}
    }

    const maybach = new Benz("silver", "maybach55");
    console.log(maybach.name);

    ```

### 이번엔 static 프로퍼티를 알아보자!

- static을 사용하면 정적멤버변수를 만들 수 있다.

```jsx
class Car {
	readonly name: string = "car";
	color: string;
	static wheels = 4;
	constructor(color: string, name) {
		this.color = color;
		this.name = name;
	}
	start() {
		console.log("start");
		console.log(this.name);
		console.log(this.wheels);  => console.log(Car.wheels);
	}
}

class Benz extends Car {
	constructor(color: string, name) {
		super(color, name);
	}
	showName() {
		console.log(super.name);
	}
}

const maybach = new Benz("silver", "maybach55");
// console.log(maybach.wheels);  // 여기서 접근하면 에러가 발생한다.
console.log(Car.wheels);

```

- 이렇게 스태틱으로 선언된 정적멤버변수는 this를 쓰는게 아니라 class명을 적어준다.
- 위처럼 class명으로 해줘야한다!

### 추상 class

```jsx
abstract class Car {
	color: string;
	constructor(color: string, name) {
		this.color = color;
	}
	start() {
		console.log("start");
	}
	//아무 작업하지 않는 메소드 생성 하면
	//abstract doSomething():void;  => 아래 class 에러 발생
}

// const car = new Car("red"); 이렇게하면 에러가 발생한다.

class Benz extends Car {
	constructor(color: string) {
		super(color, name);
	}
	showName() {
		console.log(super.name);
	}
	//abstract doSomething():void; 선언 후 오류가 발생하지 않으려면 메소드를 구현해줘야한다.
	doSomething() {
		alert(5);
	}

}

const a5 = new Benz("silver");
```

- 추상클래스는 클래스명 앞에 abstract 키워드를 사용해 선언가능하다.
- 추상클래스는 new를 이용해 객체를 만들 수는 없다. 오직 상속을 통해서만 사용이 가능하다.
- 추상클래스 내부의 추상메소드는 반드시 상속받은곳에서 구체적으로 구현을 해줘야 에러가 발생하지 않는다.
- 추상클래스는 위와 같이 프로퍼티나 메소드의 이름만 선언해주고 구체적인 기능은 상속받은 곳에서 구현해주는것을 의미한다.
- `abstract doSomething():void;` 이 추상클래스를 상속받아 만든 수많은 객체들이 동일한 메소드를 가지고 있을수 있지만 내용은 다를 수 있다는 것이다.
