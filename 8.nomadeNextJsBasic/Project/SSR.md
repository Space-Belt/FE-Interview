## SSR 만 사용하게 해주는 nextJS의 function을 알아보자!

- 지금까지는 내 page가 reload 할 때, nextJS가 page를 html 형태로 export 하던가 pre render 를 했다.
- html이 나의 app의 initial state(초기 상태) 라는것도 알고 있다. 그러므로 html 에서 loading 화면 같은것도 할수 있다.
- 그런데 nextJS가 나의 app을 pre-render 한다. 그리고 초기화면으로 html을 주는것이다.
- 그러므로 페이지를 inspect 해보면 loading이 분명 존재한다. ( 이 로딩상태는 호불호가 있다 )
- 로딩이 없길 바란다면 fetch나 서버에서 일어나는 데이터 관련된 작업을 모두한 후 API가 완료됐을때 페이지를 Render 할 것이다.

### ssr 을 사용하기 위해서는 `get server side props` 가 필요하다!

```jsx
// 바꾸기 전
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
	  );
}

// 코드 바꾸기 + 스타일
export default function Home({ results }) {
  return (
    <div className="container">
      <Seo title="Home" />
      {results?.map((movie) => (
        <div className="movie" key={movie.id}>
          <img src={`https://image.tmdb.org/t/p/w500/${movie.poster_path}`} />
          <h4>{movie.original_title}</h4>
        </div>
      ))}
      <style jsx>{`
        .container {
          display: grid;
          grid-template-columns: 1fr 1fr;
          padding: 20px;
          gap: 20px;
        }
        .movie img {
          max-width: 100%;
          border-radius: 12px;
          transition: transform 0.2s ease-in-out;
          box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
        }
        .movie:hover img {
          transform: scale(1.05) translateY(-10px);
        }
        .movie h4 {
          font-size: 18px;
          text-align: center;
        }
      `}</style>
    </div>
  );
}

export async function getServerSideProps() {
  const { results } = await (await fetch(`/api/movies`)).json();
  return {
    props: {
      results,
    },
  };
}
```

- 이름은 무조건 `getServerSideProps` 이어야 한다. `export` 도 필수다.
- 안에 어떤 코드를 쓰던 코드는 server에서 돌아간다. client 쪽이 아닌 server에서만!
- 이것을 이용해 API key를 가릴 수 있다. ( 백엔드에서만 실행되기 때문 \_
- 모든 async와 fetch를 백엔드에서 해준다.
- object를 리턴하는데 props라고 불리는 key혹은 property를 가진다.
- 이 props안에 results가 들어가는 것이다.
- 그렇다면 그 데이터를 어디서 가져오는가? → `Home( {results} )` 여기서다.
- 위의 코드는 원한다면 server side를 통해 props를 page로 보낼 수있다는 것이다.

1. getServerSideProps가 서버에서 실행
2. getServerSideProps가 무엇을 return 하던, 이걸 props로 page에게 준다. `Home( {results} )`
3. `_app.js` 에서 Next.js가 Home을 받아서 render하기 위해 Component 에 넣고, Next.js가 getServerSideProps 를 호출한다. ( Next.js는 우리가 getServerSideProps를 사용할 것을 알고 API에서 뭔가를 데이터를 호출한 후 props를 return하게 된다.)
4. 그 후 Nextjs가 그 props를 `_app.js` 의 `{…pageProps}` 자리에 넣을 것이다.
5. 결국 `Home( {results} )` 에서 결과로 나타낼 것이다.

<img width="701" alt="image" src="https://user-images.githubusercontent.com/82592845/171181553-7628a75e-f65e-41f2-9a08-02a0ea2d38e8.png">

- 위의 코드를 실행하면 오직 absolute URL 만 지원된다는 오류를 확인할 수 있다.
- `/api/movies` 이 주소가 front에서만 작동하기 때문이다. (서버에서 작동하지 않는다.)
  프론트 엔드에는 이미 브라우저에 URL이 있다.
- 그러므로 아래와 같이 추가해준다.

```jsx
export async function getServerSideProps() {
  const { results } = await (
    await fetch(`http://localhost:3000/api/movies`)
  ).json();
  return {
    props: {
      results,
    },
  };
}
```

- 이렇게 실행하면 loading이 없고 소스코드를 확인해보면 코드가 훨씬 길어져있는것을 확인 할 수있다. 이제 HTML안에 react component의 render result가 들어가있다.
- 말그대로 HTML 을 확인할 수 있다!
- 하지만 API가 돌아오기 전까진 화면에 아무것도 안보일 것이다.

→ 항상 ssr을 할것인지, 아닌지를 선택할 수 있다.
