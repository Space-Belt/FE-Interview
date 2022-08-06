# 실행 컨텍스트 (Execution Context) ?

## 실행 컨텍스트의 정의

> scope, hoisting, function, closurs, this 등의 동작원리를 담고 있는 자바스크립트의 핵심이자, **코드에 제공할 환경 정보들을 모아놓은 객체이며** 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념

## 즉, 우리의 코드가 실행되는 곳은 실행 컨텍스트 내부다!

실행컨텍스는 논리적 스택 구조를 가지며, 실행되는 순서대로 콜 스택에 쌓였다, 가장 위에 쌓여있는 컨텍스트와 관련있는 코드들을 실행하는 식으로 동일한 환경과 순서를 보장한다.

## 코드실행을 위해 제공할 정보엔 어떤것들이 있을까?

### 1. 변수 : 전역,지역,매개 변수 / 객체의 프로퍼티

### 2. 함수 선언

### 3. 변수 유효범위 (scope)

### 4. this

## 조금 더 자세히 알아보자!

### Variable Object ( 변수 객체 || VO)

- 이곳에는 변수, 매개변수, 인수정보, 함수 선언 ( 표현식 제외 )

### 실행 컨텍스트는 3가지 종류지만 하나는 사용을 많이 안한다고 하니 2개만 알아보자!

### 1. 전역 컨텍스트 (Global Execution Context)

- 전역 컨텍스트에서 Variable Object는 전역변수와 전역함수를 프로퍼티로 소유한 전역객체를 가리킨다.(Global Object)
- 기본 실행 컨텍스트로 함수 외부의 코드는 전역 컨텍스트에서 실행된다.
- 여기서는 전역 컨텍스트를 생성하며, this를 전역 객체로 설정한다.

### 2. 함수 컨텍스트 (Local Execution Context)

- 함수 컨텍스트에서 Variable Object는 지역변수,내부함수, 매개변수, 인수(args) 의 정보를 배열형태로 담고 있는 객체인 argument object를 프로퍼티로 가진 활성 객체를 가리킨다.
- 함수가 호출 될 때마다 해당 함수의 실행 컨텍스트가 생성된다.
- 각각의 함수별로 실행 컨텍스가 있지만 실행 컨텍스트는 함수가 호출될 때에 만들어진다.
- 함수 실행 컨텍스트는 얼마든지 존재 가능하며, 새로운 실행 컨텍스트가 생성될 때마다 차례대로 수행된다.

## 실행컨텍스트를 구성할 때에 생기는 것들을 알아보자!

### 1. VaribableEnvironment (변수 환경)

- 현재 컨텍스트 내 식별자들의 정보
- 외부 환경 정보
- 선언 시점의 LexicalEnvironment의 스냅샷(변경사항 적용되지 않음) ( 최초 실행시의 스냅샷 )
- 스냅샷 유지를 위해 사용됨

\*스냅샷은 나중에 알아보자!

### 2. LexicalEnvironment

- 처음에는 VariableEnvironment와 같지만 변경 사항이 **실시간으로 반영**된다.
- environmentRecord와 outerEnvironmentReference로 구성되어있다.
- environmentRecord로 인해 호이스팅이 발생 (아래에서 자세히)
- outerEnvironmentReference로 인해 스코프와 스코프체인 형성
- 클로저에 중요한 개념

### 3. ThisBinding

- 식별자가 바라봐야 할 대상 객체다.

⇒ VE 와 LE는 실행 컨텍스트에서 변수의 참조드을 기억하는 환경이다.

⇒ VE는 최초 실행시의 스냅샷이고 LE는 변경사항이 실시간으로 반영된다는것이다.

⇒ 일반적으로 LE는 해당 함수가 가지는 자신의 로컨 스코프 범위다.

## LexicalEnvironment 는 2가지로 구성되어있다.

### EnvironmentRecord와 OuterEnvironmentReference다.

### EnvironmentRecord

- 컨텍스트와 관련된 코드의 식별자 정보들이 저장되는 곳이다.
- 컨텍스트 전체를 처음 → 끝 까지 살펴 순서대로 식별자를 수집한다.
- 수집된 식별자들은 **선언된 함수, 매개변수 식변자, var로 선언된 변수**의 식별자다.
- 현재 실행된 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지만 먼저 수집하기에, 변수를 인식할 때 식별자만 끌어올리고 할당 과정은 원래 자리에 순서대로 남겨놓는다.
- 코드 실행 전이지만 자바스크립트가 코드내의 변수명을 모두 알수 있는 **호이스팅**이 여기서 발생하는것이다.

### EnvironmentRecord는 구성 요소에 따라 또 나눠진다. 알아보자!

### 1. Declarative Environment Record

- 함수 선언, 변수 선언, catch 절에서 사용되는 식별자 정보를 담고 있다.
- 스코프 내에서 선언된 식별자들의 바인딩을 관리한다.
- Environment Record를 상속한 서브클래스다.
- **Function Environment Record**
  - 함수의 최상위 스코프를 나타내는데 사용되는 선언적 환경 레코드다.
  - 화살표 함수가 아니라면 **this** 바인딩을 제공한다.
  - 화살표 함수가 아니고 **super 를** 참조할 경우엔 **super** 메서드를 실행하는데 필요한 **state**를 제공한다
- **Module Environment Record**
  - module의 외부 스코프를 나타낼 때 사용하는 선언적 환경 레코드다.

### 2. Object Environment Record

- with문 과 같이 식별자를 어떤 특정 객체 A의 속성으로 취급할 때 사용된다.
- 이 경우엔 A를 binding object 라는 속성으로 정의 한다.
- Environment Record 를 상속한 서브클래스다.

### 3. Global Environment Record

- 전역 컨텍스트의 경우, object Environment Record의 binding object는 전역객체(window)를 가리킨다.
- Object, Array, Function, parseInt와 같은 모든 built-in global과, 전역 코드에서의 함수 선언, 제너레이터 선언, 변수 선언에 의해 생성된 모든 식별자 정보는 binding object, 즉 전역 객체에서 찾을 수 있다.
- 정리하자면, 선언된 식별자들의 정보를 담고 있는 일반적인 Declarative Environment Record의 역할을 전역 컨텍스트에서는 object Environment Record가 담당한다.
- Global Enviropnment Record의 declarative Environment Record 는 object Environment Record에 포함되지 않은 식별자 정보만 보유한다.

---

### 이제 OuterEnvironmentReference 를 알아보자 !

### OuterEnvironmentReference

- 호출된 함수가 설언될 때의 Lexical Environment를 참조하는 포인터이며, 스코프 체인을 가능하게 해준다.
- 스코프 체인이란 자바스크립트가 변수의 유효 범위를 검색할 때 안에서부터 밖으로 찾아가는 것을 말한다.
- 이로 인해 전역 스코프에서 선언된 변수들은 어디서도 접근이 가능한것이다.
- OuterEnvironmentReference는 현재 호출된 함수가 선언되는 시점에서의 Lexical Environment를 참조하는 포인터다.
- OuterEnvironmentReference는 연결리스트 형태를 보이며, 선언 시점의 LexicalEnvrionment라는건 결국 해당 함수가 속한 상위 스코프의 범위다.
- 함수의 OuterEnvironmentReference는 오직 자신이 선언된 시점의 LexicalEnvrionment만 참조하고 있어 가장 가까운 요소부터 위로 차례대로만 접근이 가능하다.

```jsx
var outside = function () {
  var x = 1;

  var out = function () {
    var y = 4;

    var inside = function () {
      // inside에는 x,y 없지만 스코프 체인을 통해 값에 접근
      console.log(x, y); // 1, 4
      console.dir(inside);
    };

    inside();
    //동일
    console.log(x); // 1
  };

  out();
};

outside();
```

# 실행 컨텍스트 (Execution Context) ?

## 실행 컨텍스트의 정의

> scope, hoisting, function, closurs, this 등의 동작원리를 담고 있는 자바스크립트의 핵심이자, **코드에 제공할 환경 정보들을 모아놓은 객체이며** 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념

## 즉, 우리의 코드가 실행되는 곳은 실행 컨텍스트 내부다!

실행컨텍스는 논리적 스택 구조를 가지며, 실행되는 순서대로 콜 스택에 쌓였다, 가장 위에 쌓여있는 컨텍스트와 관련있는 코드들을 실행하는 식으로 동일한 환경과 순서를 보장한다.

## 코드실행을 위해 제공할 정보엔 어떤것들이 있을까?

### 1. 변수 : 전역,지역,매개 변수 / 객체의 프로퍼티

### 2. 함수 선언

### 3. 변수 유효범위 (scope)

### 4. this

## 조금 더 자세히 알아보자!

### Variable Object ( 변수 객체 || VO)

- 이곳에는 변수, 매개변수, 인수정보, 함수 선언 ( 표현식 제외 )

### 실행 컨텍스트는 3가지 종류지만 하나는 사용을 많이 안한다고 하니 2개만 알아보자!

### 1. 전역 컨텍스트 (Global Execution Context)

- 전역 컨텍스트에서 Variable Object는 전역변수와 전역함수를 프로퍼티로 소유한 전역객체를 가리킨다.(Global Object)
- 기본 실행 컨텍스트로 함수 외부의 코드는 전역 컨텍스트에서 실행된다.
- 여기서는 전역 컨텍스트를 생성하며, this를 전역 객체로 설정한다.

### 2. 함수 컨텍스트 (Local Execution Context)

- 함수 컨텍스트에서 Variable Object는 지역변수,내부함수, 매개변수, 인수(args) 의 정보를 배열형태로 담고 있는 객체인 argument object를 프로퍼티로 가진 활성 객체를 가리킨다.
- 함수가 호출 될 때마다 해당 함수의 실행 컨텍스트가 생성된다.
- 각각의 함수별로 실행 컨텍스가 있지만 실행 컨텍스트는 함수가 호출될 때에 만들어진다.
- 함수 실행 컨텍스트는 얼마든지 존재 가능하며, 새로운 실행 컨텍스트가 생성될 때마다 차례대로 수행된다.

## 실행컨텍스트를 구성할 때에 생기는 것들을 알아보자!

### 1. VaribableEnvironment (변수 환경)

- 현재 컨텍스트 내 식별자들의 정보
- 외부 환경 정보
- 선언 시점의 LexicalEnvironment의 스냅샷(변경사항 적용되지 않음) ( 최초 실행시의 스냅샷 )
- 스냅샷 유지를 위해 사용됨

\*스냅샷은 나중에 알아보자!

### 2. LexicalEnvironment

- 처음에는 VariableEnvironment와 같지만 변경 사항이 **실시간으로 반영**된다.
- environmentRecord와 outerEnvironmentReference로 구성되어있다.
- environmentRecord로 인해 호이스팅이 발생 (아래에서 자세히)
- outerEnvironmentReference로 인해 스코프와 스코프체인 형성
- 클로저에 중요한 개념

### 3. ThisBinding

- 식별자가 바라봐야 할 대상 객체다.

⇒ VE 와 LE는 실행 컨텍스트에서 변수의 참조드을 기억하는 환경이다.

⇒ VE는 최초 실행시의 스냅샷이고 LE는 변경사항이 실시간으로 반영된다는것이다.

⇒ 일반적으로 LE는 해당 함수가 가지는 자신의 로컨 스코프 범위다.

## LexicalEnvironment 는 2가지로 구성되어있다.

### EnvironmentRecord와 OuterEnvironmentReference다.

### EnvironmentRecord

- 컨텍스트와 관련된 코드의 식별자 정보들이 저장되는 곳이다.
- 컨텍스트 전체를 처음 → 끝 까지 살펴 순서대로 식별자를 수집한다.
- 수집된 식별자들은 **선언된 함수, 매개변수 식변자, var로 선언된 변수**의 식별자다.
- 현재 실행된 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지만 먼저 수집하기에, 변수를 인식할 때 식별자만 끌어올리고 할당 과정은 원래 자리에 순서대로 남겨놓는다.
- 코드 실행 전이지만 자바스크립트가 코드내의 변수명을 모두 알수 있는 **호이스팅**이 여기서 발생하는것이다.

### EnvironmentRecord는 구성 요소에 따라 또 나눠진다. 알아보자!

### 1. Declarative Environment Record

- 함수 선언, 변수 선언, catch 절에서 사용되는 식별자 정보를 담고 있다.
- 스코프 내에서 선언된 식별자들의 바인딩을 관리한다.
- Environment Record를 상속한 서브클래스다.
- **Function Environment Record**
  - 함수의 최상위 스코프를 나타내는데 사용되는 선언적 환경 레코드다.
  - 화살표 함수가 아니라면 **this** 바인딩을 제공한다.
  - 화살표 함수가 아니고 **super 를** 참조할 경우엔 **super** 메서드를 실행하는데 필요한 **state**를 제공한다
- **Module Environment Record**
  - module의 외부 스코프를 나타낼 때 사용하는 선언적 환경 레코드다.

### 2. Object Environment Record

- with문 과 같이 식별자를 어떤 특정 객체 A의 속성으로 취급할 때 사용된다.
- 이 경우엔 A를 binding object 라는 속성으로 정의 한다.
- Environment Record 를 상속한 서브클래스다.

### 3. Global Environment Record

- 전역 컨텍스트의 경우, object Environment Record의 binding object는 전역객체(window)를 가리킨다.
- Object, Array, Function, parseInt와 같은 모든 built-in global과, 전역 코드에서의 함수 선언, 제너레이터 선언, 변수 선언에 의해 생성된 모든 식별자 정보는 binding object, 즉 전역 객체에서 찾을 수 있다.
- 정리하자면, 선언된 식별자들의 정보를 담고 있는 일반적인 Declarative Environment Record의 역할을 전역 컨텍스트에서는 object Environment Record가 담당한다.
- Global Enviropnment Record의 declarative Environment Record 는 object Environment Record에 포함되지 않은 식별자 정보만 보유한다.

---

### 이제 OuterEnvironmentReference 를 알아보자 !

### OuterEnvironmentReference

- 호출된 함수가 설언될 때의 Lexical Environment를 참조하는 포인터이며, 스코프 체인을 가능하게 해준다.
- 스코프 체인이란 자바스크립트가 변수의 유효 범위를 검색할 때 안에서부터 밖으로 찾아가는 것을 말한다.
- 이로 인해 전역 스코프에서 선언된 변수들은 어디서도 접근이 가능한것이다.
- OuterEnvironmentReference는 현재 호출된 함수가 선언되는 시점에서의 Lexical Environment를 참조하는 포인터다.
- OuterEnvironmentReference는 연결리스트 형태를 보이며, 선언 시점의 LexicalEnvrionment라는건 결국 해당 함수가 속한 상위 스코프의 범위다.
- 함수의 OuterEnvironmentReference는 오직 자신이 선언된 시점의 LexicalEnvrionment만 참조하고 있어 가장 가까운 요소부터 위로 차례대로만 접근이 가능하다.

```jsx
var outside = function () {
  var x = 1;

  var out = function () {
    var y = 4;

    var inside = function () {
      // inside에는 x,y 없지만 스코프 체인을 통해 값에 접근
      console.log(x, y); // 1, 4
      console.dir(inside);
    };

    inside();
    //동일
    console.log(x); // 1
  };

  out();
};

outside();
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efd153ac-b06f-43b8-bc18-8bb4da40fe4b/Untitled.png)

- 위 코드에서 LexicalEnvironment를 참조한다는것을 알 수 있다.
- 또한 클로저 함수임을 확인 할 수 있다.

## ThisBinding

- This는 따로 알아보자!

- 위 코드에서 LexicalEnvironment를 참조한다는것을 알 수 있다.
- 또한 클로저 함수임을 확인 할 수 있다.

## ThisBinding

- This는 따로 알아보자!
