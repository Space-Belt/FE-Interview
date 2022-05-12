# Polymorphism

- 다형성, 제네릭, 클래스, 인터페이스를 모두 합쳐 보자!
- 그 전에 다형성은 다른 모양의 코드를 가질 수 있게 해주는 것 이란 개념을 기억하자
- 그 다형성을 이루는 방법은 제네릭을 사용하는 것이다.
- 제네릭은 placeholder 타입을 사용할 수 있게 한다.
- concrete 타입이 아닌 placeholder 타입이다.
- 때가 되면, 타입스크립트가 placeholder 타입을 concrete 타입으로 바꾸어준다.
- 즉 concrete 타입을 사용할 필요 없이, 그냥 placeholder만 사용하면 된다.
- 그러면 코드도 예뻐지고 같은 코드를 다른 타입에 대해서 쓸 수 있도록 해준다.

---

### 브라우저에서 쓰는 로컬 스토리지 API와 비슷한 API를 만들어보자

- 실제로 만드는것은 아니고, 타입만 좀 작성해 볼 것이다.
- call signature 와 클래스를 만들지만, 실제로 구현하지는 않을것이다.
- 일반적인 자바스크립트에서 사용한 로컬 스토리지 API와 같은 API를 가지는 클래스를 만든다.

```jsx
// 1
interface SStorage<T> {
    [key:string]: T
}

// 2
class LocalStorage<T> {
    private storage: SStorage<T> = {}
    // 3
    set(key:string, value:T){
        if(this.storage[key]) {
            console.log("err");
            return;
        }
        this.storage[key] = value;
    }
    // 4
    remove(key:string) {
        delete this.storage[key]
    }
    get(key:string):T {
        return this.storage[key]
    }
    clear() {
        this.storage = {}
    }
}

// 5
const stringsStorage = new LocalStorage<string>()

stringsStorage.get("aa")
stringsStorage.set("hello", "aa")

const booleansStorage = new LocalStorage<boolean>()

booleansStorage.get("XXX")
booleansStorage.set("hello", true)
```

- 1
  - 타입스크립트에 의해 이미 선언된 자바스크립트의 웹 스토리지 API를 위한 인터페이스다. 다른 인터페이스를 override 하지않기 위해 만든다.
- 1, 2
  - 제네릭은 다른 타입에 물려줄 수 있다. 제네릭을 클래스로 보내고, 클래스는 제네릭을 인터페이스로 보낸 뒤에 인터페이스는 제네릭을 사용한다.
- 3
  - 3번은 API 디자인의 구현을 보여주기 위함이다. key와 value를 스토리지에 저장해주려면, 이 부분에서 `this.storage[key] = value;` 를 해준다.
- 4
  - 지우고 가져오고 비우는 메소드 생성
- 5
  - 클래스 사용!
  - 타입 명시해 줘야한다 !
  - 타입 스크립트는 제네릭을 바탕으로 call signature를 만들어준다.

### ⇒ 제네릭, 다형성, 클래스, 인터페이스를 합쳐 작업하는 방식을 알아봤다. (쉽고 좋다)

- 위에서 제네릭이 한 단계 더 전달되는 상황을 봤다. ( 2 번 )
- 이렇게 내가 원하는 모든 타입에 사용가능한 로컬스토리지 API를 시뮬레이션 해봤다!
