# Patterns

- Next.js를 이용할 때 많이 사용하는 패턴을 알아보자
- custom app component를 사용할 때 사용하는 layout 패턴을 알아보자!

### layout patterns

```jsx
// _app.js
import Layout from "../components/Layout";
import "../styles/globals.css";

export default function App({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
			// 여기
    </Layout>
  );
}

// Components/Layout.js
import NavBar from "./NavBar";

export default function Layout({ children }) {
  return (
    <>
      <NavBar />
      <div>{children}</div>
    </>
  );
}

//
```

- children 은 하나의 component를 또 다른 component 안에 넣을 때 사용 가능한 리엑트 기본 제공 prop이다.
- 위의 코드에서 (여기) 부분에 뭔가를 넣던 Layout의 children에서 보여진다.
- 많이 사용되는 패턴이다 하지만 너무 큰 \_app.js 파일은 좋지 않다.

### 페이지 Title <Head>

```jsx
// index.js
import Head from "next/head";

export default function Home() {
  return (
    <div>
      <Head>
        <title>Home | Next Movies</title>
      </Head>
      <h1 className="active">Hello</h1>
    </div>
  );
}

// about.js
import Head from "next/head";

export default function About() {
  return (
    <div>
      <Head>
        <title>Home | Next Movies</title>
      </Head>
      <h1>About</h1>
    </div>
  );
}
```

- 이렇게 임포트한 후 사용하면 된다.
- 중복 코드가 있는데 여러개가 지금은 2개지만 여러개가 있을수 있으니 중복을 없애는 방법을 알아보자!

```jsx
// Components/Seo.js
import Head from 'next/head';

export default function Seo({ title }) {
  return (
    <Head>
      <title>{title} | Next Movies</title>
    </Head>
  );
}

// index.js  /  about.js
import Head from "next/head";
import Seo from "../components/Seo";

export default function Home() {
  return (
    <div>
      <Seo title="Home" />
      <h1 className="active">Hello</h1>
    </div>
  );
}
```

- title을 받아오고 Next.js에서 Head 를 return 한다.
- 이 방법으로 더 많은 prop들을 넣어 head(SEO) component를 개인화 할 수 있다.
- prop 전달로 중복없이 잘 됐다!
