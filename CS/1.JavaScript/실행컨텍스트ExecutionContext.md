# 실행 컨텍스트 (Execution Context) ?

## 실행 컨텍스트의 정의

<br/>

> scope, hoisting, function, closurs, this 등의 동작원리를 담고 있는 자바스크립트의 핵심이자, **코드에 제공할 환경 정보들을 모아놓은 객체이며** 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념

<br/>

## <hl style="background-color:skyblue; color:black">즉, 우리의 코드가 실행되는 곳은 실행 컨텍스트 내부다! <br/> 그렇다면 코드실행을 위해 제공할 정보엔 어떤것들이 있을까?</hl>

<br/>

### 1. 변수 : 전역,지역,매개 변수 / 객체의 프로퍼티

<br/>

### 2. 함수 선언

<br/>

### 3. 변수 유효범위 (scope)

<br/>

### 4. this

<br/>

## <hl style="background-color:skyblue; color:black">조금 더 자세히 알아보자!</hl>

<br/>

### Variable Object ( 변수 객체 || VO)

<br/>

- 이곳에는 변수, 매개변수, 인수정보, 함수 선언 ( 표현식 제외 )

<br/>

### <hl style="background-color:skyblue; color:black"><b>실행 컨텍스트는 3가지 종류지만 하나는 사용을 많이 안한다고 하니 2개만 알아보자! </b></hl>

<br/>

### 1. 전역 컨텍스트 (Global Execution Context)

<br/>

- 전역 컨텍스트에서 Variable Object는 전역변수와 전역함수를 프로퍼티로 소유한 전역객체를 가리킨다.(Global Object)
- 기본 실행 컨텍스트로 함수 외부의 코드는 전역 컨텍스트에서 실행된다.
- 여기서는 전역 컨텍스트를 생성하며, this를 전역 객체로 설정한다.

<br/>

### 2. 함수 컨텍스트 (Local Execution Context)

<br/>

- 함수 컨텍스트에서 Variable Object는 지역변수,내부함수, 매개변수, 인수(args) 의 정보를 배열형태로 담고 있는 객체인 argument object를 프로퍼티로 가진 활성 객체를 가리킨다.
- 함수가 호출 될 때마다 해당 함수의 실행 컨텍스트가 생성된다.
- 각각의 함수별로 실행 컨텍스가 있지만 실행 컨텍스트는 함수가 호출될 때에 만들어진다.
- 함수 실행 컨텍스트는 얼마든지 존재 가능하며, 새로운 실행 컨텍스트가 생성될 때마다 차례대로 수행된다.
