# catch-all URL

- 무엇이든 캐치해내는 URL이다!
- ex) /a/b/c/d/f/g/h 이런식으로 무한대로 캐치 가능
- URL에 영화제목을 넣어주면 SEO에 매우 좋아진다.
- 이렇게 하면 유저가 홈페이지에서 클릭을 통해 상세페이지에 들어가지 않더라도 영화제목을 볼 수있다. ⇒ 영화 제목을 URL에서 가져올 것이기 때문.
- 지금은 `[id].js` 에서 단 하나의 변수만 받는다 이것을 바꿔주자!

### `[...id].js`

- spread 를 해서 무한대로 받을 수 있도록 했다.
- [`http://localhost:3000/movies/Morbius/526896`](http://localhost:3000/movies/Morbius/526896) 이 url 로 들어가도 detail 페이지에 들어갈 수있게 됐다!
- router를 한번 확인해보자!

    <img width="480" alt="image" src="https://user-images.githubusercontent.com/82592845/171821519-3936aeea-15bc-4034-aa6f-0a443ae2a023.png">

  - 이제 query는 string 값이 아닌 배열이 됐다.
  - 두개의 id가 있는데 영화 제목과, id 가 들어가 있다.
  - `[`/movies/Morbius/526896/4/4/5/6/`](http://localhost:3000/movies/Morbius/526896) 이런식으로 이상한 URL을 만들어도 다 인식해서 가져온다.

- 이 기능은 가끔 유용하게 쓰이는 기능이다!
    <img width="416" alt="image" src="https://user-images.githubusercontent.com/82592845/171821540-74ad22b0-394d-4df3-a263-b785a00585f3.png">

- 여기세 id라고 하는것은 의미가 맞지 않으니 variables나 parameters 같은 다른 것으로 바꿔줄 수 있다.

### `[...params].js`

- 이제 query.params 이런식으로 불러올 수 있으며 배열 형식이다.

### 이제 URL 을 영화제목 / 영화 ID 가 되도록 해보자 . 영화제목은 유저에게 보여질 데이터이므로 Loading을 보여주지 않아도 된다 ( URL로 직접 들어왔을때 )

- URL에 있는 영화의 번호를 API에 요청을 보내보자!
  ```jsx
  //Home 의 onClick 변경

  const onClick = (id, title) => {
    router.push(`/movies/${title}/${id}`);
  };

  // Homt 의 <Link> 변경\

  <Link href={`/movies/${movie.original_title}/${movie.id}`}>
    <a>{movie.original_title}</a>
  </Link>;
  ```
  - url에 title과 id가 들어가게 해줬다.
  - 이제 url 에 영화 제목과 번호 둘다 들어간다.
- 이제 Detail 페이지에서 ES6 문법을 사용할 수 있다.
  ```jsx
  // [...params].js

  export default function Detail() {
    const router = useRouter();
    // 여기!
    const [title, id] = router.query.params;
    return (
      <div>
        <h4>{router.query.title || "Loading..."}</h4>
      </div>
    );
  }
  ```
  - 여기! 에서 router.query.params가 두개의 요소를 가지는 배열인것을 알기에 이렇게 활용이 가능하다.
  - 이렇게 하면 Loading이 뜨지 않고 잘 작동한다.
  - URL의 영화제목은 encode 되어있다. ex ) [`http://localhost:3000/movies/Fantastic Beasts: The Secrets of Dumbledore/338953`](http://localhost:3000/movies/Fantastic%20Beasts:%20The%20Secrets%20of%20Dumbledore/338953)
  - 하지만 시크릿모드 ( incognito 모드) 로 접속하면 에러가 발생한다 !
  - 에러가 발생하는 이유는 페이지가 백엔드에서 pre-render 되기 때문이다. ( 즉 server에서 미리 렌더링 되기 때문 또한 server에는 router.query.params가 아직 존재하지 않기 때문 ⇒ 아직 서버에서 배열이 아니다. ) 그러므로 여기 ! 부분을 좀 바꿔주자
    `const [title, id] = router.query.params || [];`
    - 이제 에러가 사라진다. ⇒ 이것은 그냥 client-side rendering만 해준것이다. 검색엔진은 **Doctor Strange in the Multiverse of Madness** 이런 텍스트를 찾을 수 없다( 소스 코드에도 찾을 수 없다. 그냥 네비게이션과 빈 div, 빈 h4 태그만 확인 가능하다 )
    - 그러므로 CSR 이 아니게 하는 법을 아래서 알아보자 ( SEO에 좋은 SSR )
  ### getServerSideProps()
  ### 1)
  ```jsx
  // [...params].js

  export function getServerSideProps(ctx) {
    console.log(ctx);
    return {
      props: {},
    };
  }
  ```
  - 일단은 빈 props 객체를 반환해볼 것이다. 또한 Next.js가 server-side context ( ctx ) 를 제공해주는데 콘솔로 확인해보자!
      <img width="582" alt="image" src="https://user-images.githubusercontent.com/82592845/171821574-9aaec1f2-ec61-4ff6-8704-05d02af6a0ba.png">

  - ctx를 확인해보니 server에 params가 있는것을 확인 가능하다.
  - 만약 유저에게 로딩단계를 보여주고 싶지 않고 SEO에 최적화 시키고 싶다면 getServerSideProps 를 사용하자!
  - 이 경우에선 단순히 영화제목과 ID를 얻기 위해 getServerSideProps 를 사용 가능 하다.
  - API로 데이터를 fetch 해오기 위함이 아니다!
  ```jsx
  import { useRouter } from "next/router";

  export default function Detail({ params }) {
    const router = useRouter();
    const [title, id] = params || [];
    return (
      <div>
        <h4>{title || "Loading..."}</h4>
      </div>
    );
  }

  export function getServerSideProps({ params: { params } }) {
    return {
      props: {
        params,
      },
    };
  }
  ```
  - 이제 context 안에서 params를 가져오고 props에 params을 넣어줬다.
  - Detail 에서는 params 를 받고 `router.query.params => params` 해준다.
  - 이제 소스코드에서 백엔드에서 가져온 데이터를 확인 가능하다!

### 정리 하자면 컴포넌트 내부에서 router를 사용시 router는 프론트에서만 실행이 된다. 그러므로 소스코드에서 확인할수 있는것이 없다.

getServerSideProps 를 사용해 URL에 있는 정보만 가져오고 API fetch 하는데 사용하지 않았다.
index.js에서는 API Fetch를 했다.
