> <h1>목표!</h1>

> <h3>반응형 웹</h3>

> <h3>scroll에 따라서 nav바 상단고정 , 로고 nav에 추가 기능 만들기!</h3>

> <h3>즉, navbar 형태변화를 만져본다!</h3>

<img alt="image" src="https://user-images.githubusercontent.com/82592845/163195136-ffefe587-1e58-4807-a6cd-0657c891a001.png">

<img alt="image" src="https://user-images.githubusercontent.com/82592845/163195293-b259cf10-ed89-45fb-9f8e-b61c4a8f77bc.png">

## 1. NavBar의 위치와 사용해야하는 컨테이너의 상단 부분을 잡는다.

```jsx
const nav = document.querySelector("#main");
const topOfNav = nav.offsetTop;
```

- 전에 사용했던 `offsetTop` 👍

## 2. NavBar가 어떻게 변할지 css 추가

```jsx
body.fixed-nav nav { position: fixed; box-shadow: 0 5px 0 rgba(0,0,0,0.1); }
```

- position을 fixed로 고정하는 css 추가

## 3. JS 사용하여 .fixed-nav를 추가 또는 제거 하도록 한다.

```jsx
function fixNav() { console.log(topOfNav) if(window.scrollY >= topOfNav) {
document.body.classList.add('fixed-nav') } else {
document.body.classList.remove('fixed-nav') } }
window.addEventListener('scroll', fixNav)
```

- 스크롤 내린값이 nav바의 위부분과 같거나 커지면 fixed-nav를 추가하고 아니면 제거하게 한다.

<aside>
💡 이렇게까지만 하면 아래의 여백부분이 자동으로 사라지거나 자동으로 생긴다.

</aside>

<img alt="image" src="https://user-images.githubusercontent.com/82592845/163195505-75202f9d-620a-419b-99ee-185fb5830d19.png">

- 이 현상은 nav바가 fixed 되면서 더이상 nav만큼의 공간을 차지하지 않기 때문이다.(float처럼 뜨는것 같다)
- 이 현상을 상쇄하기 위해 body에 padding을 주는 방법이 있다. 아래에서 실행해보자 !

## 4. 이상현상 해결하기!

```jsx
function fixNav() {
  //console.log(topOfNav); 네브 상단위치를 확인후
  if (window.scrollY >= topOfNav) {
    document.body.style.paddingTop = nav.offsetHeight + "px";
    document.body.classList.add("fixed-nav");
  } else {
    document.body.style.paddingTop = 0;
    document.body.classList.remove("fixed-nav");
  }
}
```

- `document.body.style.paddingTop = nav.offsetHeight + 'px';`
  를 사용하여 nav만큼의 높이를 추가해서 이상하게 줄어들지 않아 보이게 한다.
- `document.body.style.paddingTop = 0;`
  를 사용해 다시 되돌린다.

## 5. 이제 마지막으로 아래의 LOST. 를 만들어보자

<img alt="image" src="https://user-images.githubusercontent.com/82592845/163195541-eb051d19-836f-4c56-b365-a0e32452f1e7.png">

### 1. 일단 css!

```jsx
li.logo { max-width: 0; overflow: hidden; background: white; transition: all
0.5s; font-weight: 600; font-size: 30px; } /* 추가 */ .fixed-nav li.logo { /*
transition 에서 0 에서 auto로 바뀔수 없다(?) */ max-width: 500px; }
```

- max-width:가 0 이기 떄문에 fixed 되면 500px로 변경해주기만 하면된다!

# 6. 스크립트코드 완성본

### HTML

```jsx
<script>
    const nav = document.querySelector('#main');
    const topOfNav = nav.offsetTop;


// window.addEventListener('scroll', fixNav)
    function fixNav() {
        //console.log(topOfNav); 네브 상단위치를 확인후
        if(window.scrollY >= topOfNav) {
            document.body.style.paddingTop = nav.offsetHeight + 'px';
            document.body.classList.add('fixed-nav');
        } else {
            document.body.style.paddingTop = 0;
            document.body.classList.remove('fixed-nav');
        }
    }

    window.addEventListener('scroll', fixNav);
</script>
```
