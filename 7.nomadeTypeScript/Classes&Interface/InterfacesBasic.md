# Interfaces

---

```jsx
type Words = {
    [key:string]: string
}

class Dict {
    private words: Words
    constructor() {
        this.words = {}
    }
    add(word:Word) {
        if(this.words[word.term] === undefined) {
            this.words[word.term] = word.definition;
        }
    }

    update(term:string, wordss:string) {
        if(this.words[term] !== undefined) {
            this.words[term] = wordss;
        }
    }

    delete(word:Word) {
        if(this.words[word.term] !== undefined) {
            // this.words[word.term] = undefined
            delete this.words[word.term];
        }
    }
    definition(term:string){
        return this.words[term]
    }
}

class Word {             // 여기 !!!
    constructor(
        public term:string,
        public definition: string,
    ) {}
}

const tomato = new Word("tomato", "야채");
const apple = new Word("apple", "사과");

const dict = new Dict()

dict.add(tomato);
dict.definition("tomato");
dict.update("tomato", "토마토마토")
dict.delete(tomato);
```

- 여기!!! 서는 add에서 단어를 추가할 때 word를 보내려면 term과 definition 두 가지 프로퍼티들을 public이어야하기에 public으로 선언했다.
- 하지만 `tomato.definition = "xxxx"` 이렇게 누군가가 단어의 내용을 수정하지 못해야한다.
- 그렇다면 public 이지만 더 이상 변경할 수 없도록 하려면 어떻게 해야할까? ⇒ 즉, 값을 보여줄수는 있지만 수정은 불가능하게 하고 싶다!
- 이때 property를 readonly로 바꾸어 주면 된다.
  ```jsx
  class Word {
      constructor(
          public readonly term:string,
          public readonly definition: string,
      ) {}
  }
  ```
- 그러면 public 이더라도 `tomato.definition = "xxxx"` 이런식의 수정이 불가능해진다.
- 당연히 자바스크립트로 컴파일된 코드에서는 readonlyr가 보이지 않는다.

---

- 앞서 static은 언급하지 않았는데 static메소드는 타입스크립트의 것이 아니다.

```jsx
class Dict {
  static hello() {
    return "hello";
  }
}

Dict.hello(); // 작동한다!
```

- 이렇게 테스트 해보면 static은 자바스크립트의 것이라는것을 알 수 있다.

---

### Interfaces에 대해 알아보자!

- 인터페이스는 타입과 비슷하지만, 두 가지 부분에서 차이점이 존재한다.
- 우선 타입스크립트에서 type을 사용하는것이 얼마나 이로운지 알아야한다.
  - 내가 원하는대로 type을 지정할 수 있다.
  ```jsx
  type Player = {
    nickname: string,
    healthBar: number,
  };
  const woo: Player = {
    nickname: "woo",
    healthBar: 10,
  };
  ```
  - 여기서 타입을 지정하는것은 타입스크립트에게 object의 모양을 알려주는것이다.
  - 타입을 사용하는 한가지 방법이었고, 다른 방법을 보자!
  ```jsx
  type Food = string;

  const tomato: Food = "delicious";
  ```
  - Food가 string이기에 잘 작동한다.
  - 알리아스를 사용할 수도 있다.
  ```jsx
  type Nickname = string;
  type Health = number;
  type Friends = Array<string>;

  type Player = {
    nickname: Nickname,
    healthBar: Health,
  };
  const woo: Player = {
    nickname: "woo",
    healthBar: 10,
  };
  ```
- 아직 다루지 않았지만 타입을 지정된 옵션으로만 제한할 수도 있다.
  ```jsx
  type Team = "red" | "blue" | "Yellow";
  type Health = 1 | 5 | 10;

  type Player = {
    nickname: string,
    team: Team,
    health: Health,
  };

  const woo: Player = {
    nickname: "woo",
    team: "red",
    health: 1,
  };
  ```
  - 이렇게 concrete 타입의 특정 값을 쓰는것도 가능하다.
  - 선택지에 있는 것들만 들어가야한다.

### ⇒ 즉 타입은 오브젝트의 모양을 묘사하는데 사용도 가능하고, 타입 alias를 만드는데도 사용이 가능하다. 또한 타입이 특정 값을 가지도록 제한할 수도 있다.

---

### 이제 오브젝트의 모양을 묘사하는 다른 방법인 인터페이스에 대해 알아보자!

- 차이점이 있긴하지만 거의 비슷하다.

```jsx
type Team = "red" | "blue" | "Yellow";
type Health = 1 | 5 | 10;

interface Player {
  nickname: string;
  team: Team;
  health: Health;
}

const woo: Player = {
  nickname: "woo",
  team: "red",
  health: 1,
};
```

- 타입은 내가 원하는대로 가능하다. Team은 string의 배열이 될수도 있으며, 특정 값이 될 수도 있다.
- 그리고 Player타입처럼 object 모양을 특정하는데 사용도 가능하다.
- 인터페이스는 오직 한가지 용도만 있다.
- 그것은 바로 오브젝트의 모양을 특정해주는 것이다.
  - 인터페이스는 React.js를 이용해 작업을 할 때 많이 사용할 것이다.
- 타입스크립트에게 오브벡트의 모양을 알려주는 방법인 두가지중 하나이다.
- 다른점은 type키워드는 interface 키워드에 비해 좀 더 활용할 수 있는 게 많다는 것이다.
- 인터페이스는 `interface Hello = string` 이런것이 불가능하다.
  - 오직 오브젝트의 모양을 타입스크립트에게 설명해주기 위해서만 사용되는 것이다.
- 인터페이스를 다루는것이 클래스를 다루는 듯한 느낌이라 더 쉽게 느껴질 수 있다.

```jsx
interface User {
  name: string;
}

interface Player extends User {}

const woo: Player = {
  name: "woo",
};
```

- 보다시피 User를 상속받은 부분은 클래스와 매우 유사하다. (객체지향 프로그래밍)
- 인터페이스는 오직 타입스크립트에게 오브젝트의 모양을 설명하기 위해 존재하는것을 잊지말자
- 이 작업을 타입으로도 가능한데 알아보자!

```jsx
type User = {
  name: string,
};

type Player = User & {};

const woo: Player = {
  name: "woo",
};
```

- 똑같이 작동한다!!
- & 연산자를 사용했다. ( and )

```jsx
interface User {
		readonly age:number
}

interface User {
    name:string
}

interface User{
    lastName:string
}

interface User{
    health:number
}

const woo: User = {
    name:"woo",
    lastName:"woo",
    health:10,
		age:100
}
```

- 이렇게 readonly도 사용가능하다.
- 또한 여러개의 interface를 만들어서 property들을 축적시킬수 있다.
- 타입으로는 불가능하다.
