# Routing

### 예시로 알아보자

1. components 폴더 생성 후 파일 NavBar.js 생성

```jsx
// *********** NavBar.js ***********//

export default function NavBar() {
  return (
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
  );
}

// *********** index.js ***********//
import NavBar from "../components/NavBar";

export default function Home() {
  return (
    <div>
      <NavBar />
      <h1>Hello</h1>
    </div>
  );
}
```

- 이대로 하면 작동은 하지만 NavBar.js 의 anchor `<a>` 태그를 네비게이팅하는데 사용하지 말라는 에러(? 밑줄?!?) 이 생긴다. (ESLint)
  <img alt="image" src="https://user-images.githubusercontent.com/82592845/169993878-98e3a17c-c07b-44de-a91c-7f851434d7dd.png">
- NextJS 어플리케이션에서 anchor 태그를 네비게이팅할때 사용하면 안되는 이유는 NextJS에, 앱 내에서 페이지를 네비게이트 할 때 사용해야만 하는 특정 컴포넌트가 있기 때문이다.
- ReactJS에서 React Router Link를 사용해야만 할 때와 같은 이유다. → 페이지에 들어가면 어플리케이션은 전체 새로고침된다. 즉 클라이언트 사이드 네비게이션이 없다는 뜻이다.
- 전체를 새로고침하면 느리기 때문에, NextJS에서는 특정 LInk 컴포넌트가 존재한다.

### Link를 알아보자 !

- Link는 NextJS 어플리케이션의 클라이언트 사이드 네비게이션을 제공한다.

```jsx
import Link from "next/link";
export default function NavBar() {
  return (
    <nav>
      <Link href="/">
        <a>Home</a>
      </Link>
      <Link href="/about">
        <a>About</a>
      </Link>
    </nav>
  );
}
```

- 확실히 빨라졌다.
- 웹사이트를 새로고침할 필요가 없다.
- `<a>` 태그가 없어도 되나 Link에 style, className 같은 속성을 줄 수 없다.
- `<Link>` 는 오직 href 에만 필요하고 나머지는 프로퍼티는 그대로 `<a>` 태그안에 다 넣어줘야 한다.

### Router와 연결되는 Hook ( useRouter )

```jsx
import Link from "next/link";
import { useRouter } from "next/router";

export default function NavBar() {
  const router = useRouter();
  // console.log(router); 우리 위치의 정보를 얻는다. asPath 등등
  // 내가 어느 페이지에 있는지 알고 싶다면?
  return (
    <nav>
      <Link href="/">
        <a style={{ color: router.pathname === "/" ? "red" : "blue" }}>Home</a>
      </Link>
      <Link href="/about">
        <a style={{ color: router.pathname === "/about" ? "red" : "blue" }}>
          About
        </a>
      </Link>
    </nav>
  );
}
```

- Router에 hook을 걸어서 내 페이지가 어딘지 style로 나타냈다.
- Router설치 x , Router Rendering x → 그냥 작동하는것이다. (NavBar 컴포넌트 안에서 동작한다.)
