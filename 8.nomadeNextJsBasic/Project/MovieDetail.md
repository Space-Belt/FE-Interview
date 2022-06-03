# Movie Detail

- 상세 보기로 가는 방법은 매우 쉽다! 아래를 따라하자!

```jsx
export default function Home({ results }) {
  return (
    <div className="container">
      <Seo title="Home" />
      {results?.map((movie) => (
        <Link href={`/movies/${movie.id}`} key={movie.id}>
          <a>
            <div className="movie" key={movie.id}>
              <img
                src={`https://image.tmdb.org/t/p/w500/${movie.poster_path}`}
              />
              <h4>{movie.original_title}</h4>
            </div>
          </a>
        </Link>
      ))}
    </div>
  );
}
```

- Link 를 next/link 에서 임포트 해준다.
- href 를 해준다! key를 넣어준다!
- 하지만 div를 a태그 안에 넣으면 안된다는 말이 있다. (블록태그안에 스팬태그 x (html 스펙))

### ⇒ 그러므로 Link 태그로 영화 제목 텍스트를 감싸자!

```jsx
export default function Home({ results }) {
  return (
    <div className="container">
      <Seo title="Home" />
      {results?.map((movie) => (
        <div className="movie" key={movie.id}>
          <img src={`https://image.tmdb.org/t/p/w500/${movie.poster_path}`} />
          <h4>
            <Link href={`/movies/${movie.id}`}>
              <a>{movie.original_title}</a>
            </Link>
          </h4>
        </div>
      ))}
    </div>
  );
}
```

- 사진을 클릭하면 들어가지지 않지만 제목을 클릭하면 들어가진다.

### ⇒ 이제 다른방법으로 navigating 을 해보자! 그건 바로 ! router hook 이다!

```jsx
export default function Home({ results }) {
  const router = useRouter();
  const onClick = (id) => {
    router.push(`/movies/${id}`);
  }
  return (
    <div className="container">
      <Seo title="Home" />
      {results?.map((movie) => (
        <div onClick={()=>onClick(movie.id)} className="movie" key={movie.id}>
          <img src={`https://image.tmdb.org/t/p/w500/${movie.poster_path}`} />
          <h4>
            <Link href={`/movies/${movie.id}`}>
              <a>{movie.original_title}</a>
            </Link>
          </h4>
        </div>
      ))}
		</div>
}
```

- 잘 작동한다!
- Link 로 navigating 하거나 form같은걸 제출하고 나면 코드를 통해 click을 기다리지 않고 자동으로 유저를 navigating 하고싶을 수도 있다.
- API 요청을 할때에 필요한 URL은 `/movie/{movie_id}` 다 .

### 이제 API에 rewrite를 하나 더 생성해줄것이다.

```jsx
// next.config.js

const API_KEY = process.env.API_KEY;

module.exports = {
  reactStrictMode: true,
  async redirects() {
    return [
      {
        source: "/old-blog/:path",
        destination: "/new-sexy-blog/:path",
        permanent: false,
      },
    ];
  },
  async rewrites() {
    return [
      {
        source: "/api/movies",
        destination: `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}`,
      },
      {
        source: "/api/movies/:id",
        destination: `https://api.themoviedb.org/3/movie/:id?api_key=${API_KEY}`,
      },
    ];
  },
};
```

- source 의 :id 와 destination url의 :id 동일해야한다! (이름은 자유)
- 비디오 번호와 url 에 맞게 입력해보자 ! [`http://localhost:3000/api/movies/752623`](http://localhost:3000/api/movies/752623)

<img width="1235" alt="image" src="https://user-images.githubusercontent.com/82592845/171642481-7aee0933-84f4-4dd8-b6ef-bbc20e932c02.png">

- 이후에 크롬 확장 프로그램인 Jsonview 를 설치하여 보기 좋게 해놨다.
- [`http://localhost:3000/api/movies/752623`](http://localhost:3000/api/movies/752623) 이렇게 우리의 요청이 마스킹 되기도 했다.

### 이제 Next.js의 router 시스템에 대해 더 알아보자!

- 앞서 진행했던 방식인 router.push 와 Link 외에도 URL에 정보를 숨겨 보낼수 있다.
- 우리는 이미 영화의 타이틀, 표지사진 등을 다 가지고 있다. 이론상으론 이제 영화를 클릭하면 API 에 요청을 보내 클릭된 해당 영화의 정보를 받아 영화의 제목과 이미지를 사용하게 될것이다.
- 하지만 그 데이터는 우리가 이미 React.js 데이터로 가지고 있다.
- 그러므로 이제 URL에서 URL로 state를 넘겨주는 방법을 배워보자! 그리고 그것을 유저로부터 숨기는 방법도 알아 볼 것이다.

1. push 를 할때엔 선택지가 존재한다. URL을 string으로 보낼수도 있고 객체로 보내줄수도 있다.

   <img width="575" alt="image" src="https://user-images.githubusercontent.com/82592845/171801456-1dccb06f-3adc-4da4-a8c7-145fe7444f82.png">

   - push 에 호버를 해보면 **as** 를 확인할 수 있는데, 필수는 아니지만 이것을 사용하면 브라우저의 URL을 마스킹한다. ( 아래에서 차근차근 알아보자 )

   ```jsx
   export default function Home({ results }) {
     const router = useRouter();
     const onClick = (id) => {
       router.push({
         pathname: `/movies/${id}`,
         query: {
           id,
           title: "potatos",
         },
       });
     };
     return (
       <div className="container">
         <Seo title="Home" />
         {results?.map((movie) => (
           <div
             onClick={() => onClick(movie.id)}
             className="movie"
             key={movie.id}
           >
             <img
               src={`https://image.tmdb.org/t/p/w500/${movie.poster_path}`}
             />
             <h4>
               <Link href={`/movies/${movie.id}`}>
                 <a>{movie.original_title}</a>
               </Link>
             </h4>
           </div>
         ))}
       </div>
     );
   }
   ```

   1. 일단 as를 사용하지 않고 id 와 title을 받아왔다.

   - 이제 홈에서 영화들 중 하나를 클릭해보면 [`http://localhost:3000/movies/526896?id=526896&title=potatos`](http://localhost:3000/movies/526896?id=526896&title=potatos) url을 확인할 수 있다.  
     해당 영화 id 경로로 들어왔으며 영화에 대한 정보를 담고 있는 query를 확인 가능하다.
   - 하지만 아이디는 굳이 필요없으니 id 는 삭제 하고 다음을 진행하자.

   1. [http://localhost:3000/movies/526896?title=potatos](http://localhost:3000/movies/526896?id=526896&title=potatos) url 이 이렇게 우리가 지정한 title 이름으로 나온다.

      - 우리가 지정한 title에 클릭한 실제 영화 제목을 넣어볼것이고, 이 url은 보기 좋지 않기에 그것도 수정해보자!

      ```jsx
      const onClick = (id) => {
        router.push(
          {
            pathname: `/movies/${id}`,
            query: {
              title: "potatos",
            },
          },
          `/movies/${id}`
        );
      };
      ```

      - 1. URL을 설정하고 정보를 얹어주는 부분과, 2) 유저에게 필요없는 url을 마스킹하는 부분으로 나뉘었다.
      - 이렇게하면 브라우저에서 마스킹이 되어있다. 하지만 router(콘솔)를 들여보면 우리가 담아준 데이터를 확인 가능하다. router → query 에 있다.
      - 즉 클릭한 영화의 정보를 URL을 통해 보낼 수 있다! 또한 url을 숨길 수 있다.
        ⇒ 클릭을 할때마다 영화제목도 넘겨줄 수 있다.
      - 아래에서 넘겨줘 보자!

      ```jsx
      export default function Home({ results }) {
        const router = useRouter();
        const onClick = (id, title) => {
          router.push(
            {
              pathname: `/movies/${id}`,
              query: {
                title,
              },
            },
            `/movies/${id}`
          );
        };
        return (
          <div className="container">
            <Seo title="Home" />
            {results?.map((movie) => (
              <div
                onClick={() => onClick(movie.id, movie.original_title)}
                className="movie"
                key={movie.id}
              >
                <img
                  src={`https://image.tmdb.org/t/p/w500/${movie.poster_path}`}
                />
                <h4>
                  <Link href={`/movies/${movie.id}`}>
                    <a>{movie.original_title}</a>
                  </Link>
                </h4>
              </div>
            ))}
          </div>
        );
      }
      ```

      - onClick 에서 `movie.id, movie.original_title` 이 두가지를 넘겨줘서 실행해보면 이제 router / query 에서 영화제목을 확인할 수 있다.
      - 즉 상세페이지에서 유저에게 영화제목을 보여줄 수 있게 됐다! ( 아래에서 해보자! )

      ### [id].js

      ```jsx
      import { useRouter } from "next/router";

      export default function Detail() {
        const router = useRouter();
        return (
          <div>
            <h4>{router.query.title || "Loading..."}</h4>
          </div>
        );
      }
      ```

      - router의 querty의 title 을 보여주거나 로딩을 보여주게 했다.

      → 이제 홈에서 클릭할 때만 Detail 페이지가 작동한다. ( 제목만 보이게 했다 ) 하지만 유저가 익명모드 ( 크롬 시크릿 창)로 홈페이지를 거치지 않고 상세 페이지로 바로 접속할때는 로딩이 보이는 문제가 발생한다.

      → 크롬 시크릿 창으로 접속하면 router.query.title이 존재하지 않기 때문이다. 즉 유저가 홈페이지에서 클릭으로 상세페이지로 넘어갈때만 존재한다는 뜻이다.

### 위에서 진행된 과정은 Link 태그에도 동일하게 작동한다.

```jsx
export default function Home({ results }) {
  const router = useRouter();
  const onClick = (id, title) => {
    router.push(
      {
        pathname: `/movies/${id}`,
        query: {
          title,
        },
      },
      `/movies/${id}`
    );
  };
  return (
    <div className="container">
      <Seo title="Home" />
      {results?.map((movie) => (
        <div
          onClick={() => onClick(movie.id, movie.original_title)}
          className="movie"
          key={movie.id}
        >
          <img src={`https://image.tmdb.org/t/p/w500/${movie.poster_path}`} />
          <h4>
            <Link
              href={{
                pathname: `/movies/${movie.id}`,
                query: {
                  title: movie.original_title,
                },
              }}
              as={`/movies/${movie.id}`}
            >
              <a>{movie.original_title}</a>
            </Link>
            {/* <Link href={`/movies/${movie.id}`}>
              <a>{movie.original_title}</a>
            </Link> */}
          </h4>
        </div>
      ))}
    </div>
  );
}
```

- Link 태그에도 위와 같이 pathname과 title을 수정외에 동일하게 작성했고 “as” 를 직접 써줬다.
