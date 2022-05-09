# Classes 복습

---

- 클래스를 사용할 때 TypeScript와 JavaScript가 어떻게 다른지 알아봤다. ( protected, private, public 필드 등을 사용 했다 )
  - 또한 Constructor를 만들때 JavaScript를 사용할 때 필요한 this를 쓰지 않아도 됐다.
  - 그리고 빌드가 어떤 접근제어자(보호 등급)인지, 이름, 그리고 타입을 써주기만하면 됐다.
- private, protected, public 무엇을 사용하건 JavaScript에서는 보이지 않는다. (타입스크립트에서만 보임)
- 다음으로 우리는 클래스의 상속을 볼 수 있었다.
  - 이것은 TypeScript의 것뿐 아니라 JavaScript에서도 볼 수 있는 것이다.
  - 즉 타입스크립트에서 추상 클래스를 사용할 수 있도록 해준다.
  - 추상 클래스는 직접적으로 인스턴스를 만들지는 못하는 클래스지만 그 클래스를 상속할 수 있다.
  - 만약 추상 클래스에서 어떤 메소드를 구현하면 상속받는 클래스는 그 메소드를 호출할 수 있다.
  - 또한 메소드를 보호할 수 있다. ( 접근제어자 )
- 추상 클래스에 대해 알아봤고 그 다음은 추상 메소드였다.
  - 추상 메소드는 구현이 되어있지 않은 메소드다 ( 코드가 없음 )
  - call signature만 가지고 있는데, 함수의 이름과 argument를 안 받을 때도 있지만 argument를 받을 경우엔 argument의 이름과 타입 그리고 함수의 리턴타입을 정의하고 있다. `abstract getName():void` 이게 추상 메소드다.
  - 추상 메소드가 있는 경우엔, 추상 클래스를 상속받는 클래스에서 추상 메소드를 구현해 줘야한다.
- 기본 값은 public이고 private와 protected가 있다. private로 바꾼다면, 상속받는 클래스에서 상속될 클래스에 접근할 수 없다. → 즉 private로 되어있는 것은 해당 클래스에서만 접근이 가능하다.
  - 모든 클래스에서 접근, 사용 가능하도록 만드려면 protected를 사용해야한다.
  - 또는 외부의 모든 곳에서 사용가능하게 하려면 public으로 해주면 된다. (안써도 기본값임)

### ⇒ 이 모든 것들은 타입스크립트에서 제공하는 것들이다.

<img width="481" alt="image" src="https://user-images.githubusercontent.com/82592845/167343354-898e4910-608f-482d-a417-b89e25860a40.png">

자바스크립트에서는 이렇게 새로운 유저가 만들어지는데 타입스크립트에서는 허용하지 않는다.

---

## 이제 실전으로 지금까지 배운 것들을 이용하여 해시맵을 만들어보자!

- 해싱 알고리즘을 쓰는 완벽한 해시맵이 될것이다.
- 단어 사전을 만들어보자!
  - 사전에 새 단어를 추가하고, 단어를 찾고, 단어를 삭제하는 메소드를 만들것이다.

```jsx
class Dict {
    private words
}
```

- words를 private로 선언할 것이다.
- 이 property는 Constructor에서 직접 초기화 되지 않는 property다.

```jsx
class Dict {
    constructor(
        private x:string
    ) {}
}

-> 아래의 자바스크립트 코드   ( 여기 !!!! )
class Dict {
    constructor(x) {
        this.x = x;
    }
}
```

- 이렇게 자바스크립트에서 자동으로 초기화가 된다.
- 아래에서 private words를 선언하고, words의 타입을 알려줘볼 것이다.

```jsx
type Words = {
    [key:string]: string
}

class Dict {
    private words: Words;
}
```

- 여기서 타입이 처음보는 방식으로 선언됐는데, Words 타입이 string 만을 property로 가지는 오브젝트라는것을 말해준것이다.
- 예를 들어 Words 타입의 dict(사전)을 만들면

```jsx
type Words = {
  [key: string]: string, // key에 아무거나 들어가도 되지만 보통 key를 많이 사용한다.
};

let dict: Words = {
  potato: "food",
};
```

- 이렇게 넣을수 있는데. 둘다 스트링이 들어가야한다.
- 이 형식은, 제한된 양의 property 혹은 key를 가지는 타입을 정의해 주는 방법이다.
- 즉 여기서 타입 선언부분은 오브젝트의 타입을 선언해야할 때 사용가능하고, 이 오브젝트는 제한된 양의 property만을 가질수 있을때 사용한다. 또한 property에 대해 미리 알수는 없지만 타입만 알고 잇을때 사용한다.

  <img width="186" alt="image" src="https://user-images.githubusercontent.com/82592845/167345355-a730350f-f79a-47da-b50d-25981838b4a2.png">

- 이렇게 오류가 발생한다.
- 이 이유는 words는 initializer가 없고 Constructor에서 정의된 sign이 아니라는 에러다.
- constructor가 words를 지정해주기를 원하지 않았기에 이렇게 하면 안되기 때문에 위의 (여기 !!!) 처럼 Constructor에 해주지 않았기 때문이다.

→ 해결하기 위해서는 words를 intializer 없이 선언해주고 Constructor에서 수동으로 초기화 시켜줘야한다.

```jsx
type Words = {
    [key:string]: string
}

class Dict {
    private words: Words //  1번
    constructor() {      //  2번
        this.words = {}
    }
    add(word:Word) {     //  5번
        if(this.words[word.term] === undefined) {
            this.words[word.term] = word.definition;
        }
    }
    definition(term:string){  //  8번
        return this.words[term]
    }
}

class Word {             //  3번
    constructor(
        public term:string,
        public definition: string,
    ) {}
}

const tomato = new Word("tomato", "야채");  //  4번

const dict = new Dict()  //  6번

dict.add(tomato);        //  7번
dict.definition("tomato");  //  9번 토마토의 definition 알아보기!
```

- 1번 words를 initializer 없이 선언해준다.
- 2번 contructor에서 수동으로 초기화시켜준다. 여기까지가 내 dict( 사전이 될것이다 )
- 3번 각각의 단어를 만들기 위해 Word 클래스를 만든다.
- 4번 새로운 단어 생성
- 5번 단어를 추가하기 위한 메소드 생성 ( 주어진 단어가 아직 사전에 없을때 값을 할당 )
- 6번 새로운 사전을 만들기
- 7번 사전에 4번에서 만든 단어 넣기
- 8번 words가 private기 떄문에 dictionary 안에서만 words를 볼 수 있게 term을 사용해 word를 찾는 메소드를 만든다.
- 9 번에서 실행된다!

- 자바스크립트로 컴파일된 코드를 실행시켜보면 잘 작동한다!
  <img width="406" alt="image" src="https://user-images.githubusercontent.com/82592845/167346174-163e4591-5f99-45b3-b5c7-c052e8a733c6.png">

---

### 배운것들!

- 위에서 처음보는 프로퍼티의 이름타입을 정하는 법을 배웠다.
- property가 constructor부터 바로 자동으로 초기화되지 않는것도 배웠다. 고로 우리가 직접 수동으로 초기화 시켜줬다. 위의 1번
- 3번을 5번에서 클래스를 만들때 클래스를 타입처럼 사용할 수 있다는것도 배웠다.
  - 파라미터 부분에 클래스를 입력해 준 적이 없지만, 클래스를 타입으로 쓸때는 가능한것이다.
  - 파라미터가 이 클래스의 인스턴스이기를 원하면 이렇게 사용이 가능하다.

### 단어 수정, 삭제를 만들어보자!

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

class Word {
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

- 성공 !
- update는 두개를 받아야한다.
- delete는 delete 메소드를 사용한다. ( [참고!](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/delete) )
