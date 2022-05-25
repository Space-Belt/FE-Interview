# CSS Modules

1. NavBar.js 를 위해 NavBar.module.css 파일 생성

   - NavBar 는 중요하지 않고 .module.css 패턴이 중요하다.

   ```jsx
   .nav {
     display: flex;
     justify-content: space-between;
     background-color: red;
   }
   ```

2. css 적용

   ```jsx
   import styles from "./NavBar.module.css"

   <nav className={styles.nav}>
   ```

   - 이게 작동하는 이유는, 우리가 CSS모듈 패턴을 사용했기에 가능하다.
   - 클래스이름을 추가할 때, 클래스이름을 텍스트로서 적지 않고, 자바스크립트 오브젝트에서의 프로퍼티 형식으로 적는다.
   - 이 접근방식의 장점으로는, 실제 이 클래스 이름을 살펴보면 이름이 다르다. (무작위 텍스트)
   - 이렇게 되면 충돌이 일어나지 않는다.
   - 즉 → 다른 컴포넌트에서도 nav라는 클래스 이름을 사용할 수 있다.
   - 클래스의 중복 걱정없이 계속해서 사용가능하다. 페이지가 빌드될 때, NextJS가 클래스 이름을 무작위로 바꿔준다.

3. 이제 class를 사용하여 해보자

   ```jsx
   <nav>
     <Link href="/">
       <a className={router.pathname === "/" ? styles.active : ""}>Home</a>
     </Link>
     <Link href="/about">
       <a className={router.pathname === "/about" ? styles.active : ""}>
         About
       </a>
     </Link>
   </nav>
   ```

   ```jsx
   .active {
     color: tomato;
   }
   ```

   - 적용이 잘됐다.

4. 두가지 클래스를 적용해보자 !

   ```jsx
   .link {
     text-decoration: none;
   }

   .active {
     color: tomato;
   }
   ```

   ```jsx
   <nav>
     <Link href="/">
       <a
         className={`${styles.link} ${
           router.pathname === "/" ? styles.active : ""
         }`}
       >
         Home
       </a>
     </Link>
     <Link href="/about">
       <a
         className={[
           styles.link,
           router.pathname === "/about" ? styles.active : "",
         ].join(" ")}
       >
         About
       </a>
     </Link>
   </nav>
   ```

   - 위의 방법 1
   - 방법 2는 `.join` 으로 한 배열을 다른 한 문자열로 바꾸는 방법으로 한다.

---

### 여기까지가 module 사용방법인데 파일을 바꿔야 하기에 별로 좋지 않은 방식일 수 있다.

- 클래스 이름 기억도 해야한다.
- 또한 클래스이름 자체를 구현하는게 꽤 복잡하다. (조건자같은것)
