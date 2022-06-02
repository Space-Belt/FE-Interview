# DynamicRoutes

### 지금까지 따라왔다면 Home 과 About 두개의 URL이 있다.

### 이제 URL에 변수를 넣는 방법에 대해 배워보자!

- Home에서 영화를 클릭하면 해당 movie(상세보기) 로 넘어가도록 해보자!
- /movies/:id(영화아이디) 이런 url 을 원한다. CRA의 react-router-dom에서는 url을 이런식으로 특정한다. 그럼 react-router-dom이 :id가 변수라는 것을 인식한다.
- Next.js에서는 다른 방법으로 한다. Next.js에는 router가 없다.

### Next.js는 폴더와 파일의 이름만을 가지고 가능하게 해준다

- /movies/all url로 간다고 가정해보면 이런식으로 된다.
    <img width="205" alt="image" src="https://user-images.githubusercontent.com/82592845/171596967-d38ad72a-f676-47bd-aa27-9020d645ded1.png">
    
    ```jsx
    // all.js
    
    export default function All() {
    	return "all";
    }
    ```

- 그냥 /movies 만 하고 싶다면 ?
  - pages/movies pages에 movies를 넣으면 된다.
  - 하지만 /movies/all 도 동시에 있다면 ??
  - movies폴더에 index.js 를 만들어준다.

### 그렇다면 Next.js에 URL에 변수가 필요한지 알려주는 방법을 알아보자!

- 이렇게 하면 /movies/123213123(변수) 이런 변수를 넣을 수 있다.
    <img width="384" alt="image" src="https://user-images.githubusercontent.com/82592845/171601331-8c11170c-e855-488a-a999-ea53fe0ab9ef.png">
    
    - `[ 여기 ]` 여기에 변수명을 아무렇게나 넣을수 있다.
    
    ```jsx
    import { useRouter } from "next/router";
    
    export default function Detail() {
      const router = useRouter();
      console.log(router);
      return "detail";
    }
    ```

- /movies/1 로 가보면 detail 으로 잘 떠있는것을 확인 할 수 있다. Next.js에 이게 변수를 포함하는 Dynamic URL 이라는 것을 알려줄 수 있는 유일한 방법은 대괄호를 사용하는 것이다.
    <img width="387" alt="image" src="https://user-images.githubusercontent.com/82592845/171601569-f79a767f-d154-46f2-a494-a85fa9534f94.png">
    
    - 콘솔에 출력된 router를 살펴보면 query가 있는데 여기 표시된 ID가 있고 URL과 동일하다.
    - 이 query 프로퍼티의 이름은 파일명에 쓰인 변수명이랑 동일하다.
