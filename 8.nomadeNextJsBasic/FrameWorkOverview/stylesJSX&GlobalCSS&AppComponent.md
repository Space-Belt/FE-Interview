# stylesJSX & GlobalCSS & AppComponent

# styles JSX

- NextJS 어플리케이션에서 styles를 추가하는 또 다른 방법
- styled jsx는 NextJS의 고유 방법이라 할 수 있다.

### 적용하기!

```jsx
// NavBar.js

<nav>
  <Link href="/">
    <a>Home</a>
  </Link>
  <Link href="/about">
    <a>About</a>
  </Link>
  <style jsx>{`
    nav {
      background-color: tomato;
    }
    a {
      text-decoration: none;
    }
  `}</style>
</nav>
```

<img width="596" alt="image" src="https://user-images.githubusercontent.com/82592845/170196963-8f90fb65-4312-4c02-a97d-74af7a7a2422.png">

- 적용 잘됐다.
- 아래의 예시를 보자

```jsx
// index.js

<div>
  <NavBar />
  <h1>Hello</h1>
  <a>안녕하세요</a>
  <style jsx>{`
    a {
      color: blue;
    }
  `}</style>
</div>
```

- 여기서 똑같이 a 에 style 을 주면 NavBar 의 a태그에는 작동하지 않고 index.js 파일에 있는 a 태그에만 적용된다.
- 그 이유는 모든게 독립되어 있기 때문이다.
- className으로 해도 같다.
- 즉 컴포넌트 내부로 범위가 한정된다. 자동으로 생성되는 클래스 이름들이 충돌로부터 방어해준다.

### Prop 을 끌고 오는 방법 !

```jsx
<style jsx>{`
  a {
    color: ${props.color};
  }
`}</style>
```

- 그냥 이런식으로 사용 가능하다.

### Global styles 적용방법

```jsx
// NavBar.js

<nav>
  <Link href="/">
    <a className={router.pathname === "/" ? "active" : ""}>Home</a>
  </Link>
  <Link href="/about">
    <a className={router.pathname === "/about" ? "active" : ""}>About</a>
  </Link>
  <style jsx>{`
    a {
      text-decoration: none;
    }
    .active {
      color: tomato;
    }
  `}</style>
</nav>

// index.js

<div>
  <NavBar />
  <h1 className="active">Hello</h1>
  <style jsx global>{`
    a {
      color: white;
    }
  `}</style>
</div>
```

- `<style jsx global>` 이렇게 global이라 처리해준다.
- 하지만 Home이 눌렸을땐 About 에 white 가 적용되지만 About 클릭됐을때 Home 에는 적용이 되지 않는다.

<img width="298" alt="image" src="https://user-images.githubusercontent.com/82592845/170186013-9a4fbf95-ed10-4e13-86d2-a8592ffae477.png">

<img width="288" alt="image" src="https://user-images.githubusercontent.com/82592845/170191560-f5eecf35-20e5-42f1-b27d-85774888d43b.png">

- About에 적용되지 않는 이유는 `/about` 에 있기 때문이다. (url)
- NextJS로 작업 할 때는 페이지를 고려해야한다. CRA로 작업할 때는 React 어플리케이션에서는 생각하지 않아도 된다.
- 우리가 About 페이지를 누르면 다른페이지에 있기 때문에 적용되지 않는것이다. 즉 렌더링 되고 있는 컴포넌트 또한 다른 컴포넌트인것이다.
- About 은 이 네비게이션 바에서 렌더링 되고 있다.
  ```jsx
  // about.js

  export default function About() {
    return (
      <div>
        <NavBar />
        <h1>About</h1>
      </div>
    );
  }
  ```
  - About은 이 네비게이션 바에서 렌더링되고 있다.
  - 어디에도 `color:white` 는 없다. Home(index.js)페이지에 있는것!
  - 즉 → 완벽하게 전역적으로 스타일링이 된것이 아니다!

### 그렇다면 정말 전역적으로 적용해보자 !

1. 한가지 방법은 index.js에 있는 코드를 복붙하는것이다. 하지만 이 방법은 별로다.
   1. 지금 우리가 하고 있는 `<NavBar />` 를 모두 써놓는 방법도 별로다.
2. 그렇다면 1번의 해결 해보자 !
   1. 우리가 만들 모든 페이지의 청사진을 커스텀할 수 있는 장소가 필요하다.
   2. 반복되는 코드를 쓰거나 하지 않도록 !

### 정답은 App Component(`_app.js`) 를 이용한다 ! ( 청사진 )

- NextJS가 모든 페이지를 렌더링할 수 있게 하는 일종의 컴포넌트의 청사진이다.
- 기본으로 프레임워크 내에 포함되어있다.
- 따로 활성시키려고 뭘 하지는 않아도 되지만 커스터마이징 하려면, 파일을 하나 만들어야한다.
- `_app.js` 무조건 이 이름이어야 한다.
- NextJS는, About이 랜더링 되기 전에, 먼저 App을 본다. 그리고 나서 다른 내용들을 렌더링한다. ( `_app.js` 에 넣어둔 청사진을 기반해서 렌더링 )
- 어떻게 페이지가 있어야하는지, 어떤 컴포넌트가 어떤페이지에 있어야하는지 등등을 설계할수 있는 청사진이라 보면 된다.

```jsx
export default function App({ Component, pageProps }) {
  return (
    <div>
      <Component {...pageProps} />
      <span> Hello</span>
    </div>
  );
}

// 어플리케이션 컴포넌트의 기본적인 기능
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

- NextJS 가 \_app.js 불러옴 → 내부의 함수를 2가지 props와 함께 불러옴 → 하나는 Component, 하나는 pageProps다.
- Component 는 Home 이나 About이 된다.
- `<span>` 태그는 어디서든 잘 보인다.

### 중복되는 것들을 없애는 방법을 알아보자!

```jsx
import NavBar from "../components/NavBar";

export default function App({ Component, pageProps }) {
  return (
    <>
      <NavBar />
      <Component {...pageProps} />
      <style jsx global>{`
        a {
          color: white;
        }
      `}</style>
    </>
  );
}
```

- 이렇게 하면 `<NavBar />` 를 여기에서만 사용하면 되고, style도 전역적으로 적용된다.

### 이제 styles/globals.css 를 전체 적용해보자

- NextJS로 앱을 만들땐 global css파일을 import할 수 없다.
  ```jsx
  import "../styles/globals.css";

  export default function Home() {
    return (
      <div>
        <h1 className="active">Hello</h1>
      </div>
    );
  }
  ```
  - 이렇게 import 하면 이런 에러를 볼 수 있다.
      <img width="527" alt="image" src="https://user-images.githubusercontent.com/82592845/170196989-99a91c44-d1b0-4456-b9e3-667b01437d25.png">

  - 커스텀App 이외의 파일들로부터는 임포트할 수 없다는 뜻이다.
  - 즉 css를 임포트하고 싶다면, 반드시 module이어야 한다!
- 하지만 커스텀 App 컴포넌트가 있는 `_app.js` 라면 모든 Global Styles를 임포트할 수 있다.
