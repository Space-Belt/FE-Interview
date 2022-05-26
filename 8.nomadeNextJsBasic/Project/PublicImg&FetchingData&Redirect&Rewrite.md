# Img on Nextjs & Fetching Data & Redirect & Rewrite

### public 파일 사용법

- next.js를 이용시 public 파일들을 쉽게 다루는게 가능하다.
  1. public 파일들을 public 디렉토리에 넣기만 하면 된다.
  2. public 디렉토리 안에 있는 파일들을 불러오기
     `<img src="/vercel.svg" />` 이런식으로하면 끝이다.
  3. 하지만 img component를 사용하지 말라는 경고가 발생한다.
     살펴 보면 대신 next image 를 사용하라고 한다. 하지만 다루지 않겠다.
     ( image component는 로컬이미지, 원격이미지를 사용할 때 좀 복잡해질 가능성이 있다 )

### API Data 불러오기

- [www.themoviedb.org](https://www.themoviedb.org/) 에서 데이터를 가져와 사용할 것이다.
- 가입하고 API 키 받기
- [https://api.themoviedb.org/3/](https://api.themoviedb.org/3/) ( base URL)
- 인기영화 받을 곳에 fetch 할것 “`/movie/popular"`

```jsx
// 간편하게 바꾸기 전
const [movies, setMovies] = useState([]);
useEffect(() => {
  (async () => {
    const response = await fetch(
      `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}`
    );
    const json = await response.json();
  })();
}, []);

// 바꾼 후
const [movies, setMovies] = useState([]);
useEffect(() => {
  (async () => {
    const data = await (
      await fetch(
        `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}`
      )
    ).json();
    console.log(data);
  })();
}, []);
```

- 이렇게 data 를 확인해보면
  <img width="437" alt="image" src="https://user-images.githubusercontent.com/82592845/170234164-d2a1e50b-8665-4986-973b-7d931c7eb78d.png">

  이런식의 데이터가 불러와 지는것을 볼 수있다.

- 우리는 results 만 필요하기 때문에 `const data =` 로 시작한 부분을 바꿔준다.

```jsx
const API_KEY = ""sds0dfisdfi9929fds9fa9g9s9daf9;

export default function Home() {
  const [movies, setMovies] = useState([]);
  useEffect(() => {
    (async () => {
      const { results } = await (
        await fetch(
          `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}`
        )
      ).json();
      setMovies(results);
    })();
  }, []);
  return (
    <div>
      <Seo title="Home" />
      {movies.map((movie) => (
        <div key={movie.id}>
          <h4>{movie.original_title}</h4>
        </div>
      ))}
    </div>
  );
}
```

- 받아오는 데이터에서 results 만 받아 setMovies로 데이터 넣어준다!
- map 으로 movie의 property 중 하나인 original_title의 정보만 뿌렸다!

### 이번엔 loading state 를 만들어 보자!

- `const [movies, setMovies] = useState()` 괄호안에 [] 를 없애면 기본으로 아무것도 없기 때문에 map 을 할 수 없기에 오류가 발생한다.
- 그렇다고 [](빈 array) 를 넣어준다면 에러가 생기진 않지만 항상 [](빈 array) 일것이다.
- 해결방법 1
  ```jsx
  <div>
    <Seo title="Home" />
    {!movies && <h4>Loading...</h4>}
    {movies?.map((movie) => (
      <div key={movie.id}>
        <h4>{movie.original_title}</h4>
      </div>
    ))}
  </div>
  ```
  - ? 로 존재여부에 따라 map을 작동시킨다.
  - 데이터가 아직 없을때는 Loading이라는 것이 보이게 한다.
  - 하지만 나한테는 Loading 이 보이지 않았다...?

## 이제 API key 를 감춰 보자!

```jsx
<div className="container">
  <Seo title="Home" />
  {!movies && <h4>Loading...</h4>}
  {movies?.map((movie) => (
    <div className="movie" key={movie.id}>
      <img src={`https://image.tmdb.org/t/p/w500/${movie.poster_path}`} />
      <h4>{movie.original_title}</h4>
    </div>
  ))}
</div>
```

- 우선 이미지를 불러올 수 있도록 했다.
- movies는 poster path 가 있는데 그것이 전체 URL이 아니기에 전체 URL 을 받기 위해선
  `<img src={`[https://image.tmdb.org/t/p/w500/${movie.poster_path}`} />](https://image.tmdb.org/t/p/w500/)`이렇게`https://image.tmdb.org/t/p/w500/` 이 부분을 먼저 해줘야한다. - movie DB에서 image 서버 같은 앞쪽 URL 을 쓰고 API response에 poster path가 들어간것이다.

### 감춰보기!

- 일단 API 키는 검사창에서 네트워크에 가보면 request한 URL같은것들이 있는데 목록중에 하나에 있다.
- 누구나 찾을수 있기에 감춰야한다!
- request에 mask를 씌우는것 같은 redirect 와 rewrite에 대해 알아보자! (nextJS 기능)

1. 우선 `next.config.js` !

   ```jsx
   /** @type {import('next').NextConfig} */
   const nextConfig = {
     reactStrictMode: true,
   };

   module.exports = nextConfig;
   ```

   - 기본형
   - 먼저 API key를 숨기지 않는 redirect 가 있다. next.js 가 redirection을 허용한다.
   - 터미널을 보면 서버가 실행되고 있다. (client and server)
   - 원한다면 여느때와 같이 redirect는 가능하다.
     ex) `[www.woojoo.com/go](http://www.woojoo.com/go)` 이런 주소 ! ( 쉬운 방법 )

   ```jsx
   module.exports = {
     reactStrictMode: true,
     async redirects() {
       // array 리턴
       return [
         // 이 array는 object를 가지고, redirection에 필요한건 이게 다다.
         {
           // 1. source를 찾는다. 유저가 어디(contact)로 간다면
           source: "/contact",
           // 2. 유저를 form이라는 destination 으로 보낸다.
           destination: "/form",
           // 3. 이 redirection이 permanet(영구적)인지 아닌지
           permanent: false,
         },
       ];
     },
   };
   ```

   - 위의 절차를 밟고 똑같이 코드를 작성했다면
   - [localhost:3000/contact](http://localhost:3000/form) ⇒ [localhost:3000/form](http://localhost:3000/form) 으로 이동한다. 404 페이지!
   - 내 website 이내에서든, 바깥에서든 redirect 할 수 있다.

   ### 여기서 끝이 아니라 pattern matching 도 지원한다.

   ```jsx
   module.exports = {
     reactStrictMode: true,
     async redirects() {
       return [
         {
           source: "/old-blog/:path",
           destination: "/new-sexy-blog/:path*",
           permanent: false,
         },
       ];
     },
   };
   ```

   - path 가 전달된다!
   - `source: "/old-blog/path"*,` `destination: "/new-sexy-blog/:path*"`이렇게 \*를 붙여주면 모든 걸 catch 한다. ⇒ 뒷부분에 작성한 모든 것들이 전달 된다. (진짜 모든것)

   > 다른 redirects 를 만들고 싶다면 그냥 {} 하나를 더 붙이면 된다.
   > Redirect 는 접속할때 URL이 바뀌는것을 확인할 수 있다.

   ### index.js 에서 fetch를 한다. 그렇게 하면서 API_KEY는 숨기려 한다.

   그러므로 rewrites를 이용해보자!

   ```jsx
   // next.config.js

   const API_KEY = "058207a0aafa96c24c87524dc1f2be48";

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
       ];
     },
   };

   // index.js  fetch 부분

   useEffect(() => {
     (async () => {
       const { results } = await (await fetch(`/api/movies`)).json();
       setMovies(results);
     })();
   }, []);
   ```

   - index.js 에서 API_KEY 와 fetch의 주소를 옮겼다.
   - source 를 index.js 의 fetch 주소에 넣었다.
   - 이때 중요한 것은 next.js 가 이 request를 masking(가림)한다는 것이다.
   - [`http://localhost:3000/api/movies`](http://localhost:3000/api/movies) 로 들어가보면 [`https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}`](https://api.themoviedb.org/3/movie/popular?api_key=$%7BAPI_KEY%7D) 이 URL을 방문 했을때 얻게되는 response 정보다. 이것들은 우리 서버 뒤에 mask 되어 있다.
   - 검사 창을 켜서 network를 보면 request는 있지만 movie DB로 가지는 않고 대신 `[http://localhost:3000/api/movies](http://localhost:3000/api/movies)` 로 간다.

   ### 이제 next.config.js에 있는 API 키를 env 로 보내보자

   ```jsx
   // next.config.js

   const API_KEY = process.env.API_KEY;

   // .env
   API_KEY = ( API 키 )

   // .gitignore
   .env
   ```

   - 이러면 API키를 노출 시키지 않고 잘 작동된다.
