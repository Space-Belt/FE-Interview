# React Native

### 리액트 네이티브에서 자바스크립트는 상대적으로 중요하지 않은 부분이다.

### 가장 중요한 것은 Bridge들을 통해 코드가 운영체제와 통신을 할 수 있도록 하는 인프라시설이 중요하다.

<img width="630" alt="image" src="https://user-images.githubusercontent.com/82592845/185795719-72acbed5-a56f-4bee-a088-b7323cd64bdb.png">

- 이런것들이 리엑트 네이티브 앱을 구성한다.
- 자바스크립트 코드만 다운받는게 아닌 이 모든 기본시설이 있는 앱을 다운 받는것이다.
- 리액트 네이티브는 shell 하고 같다.
- 자바스크립트 코드를 넣고, 그 코드는 운영체제와 소통한다. 그러므로 Java가 설치되어 있어야한다. XCode 도 있어야한다!
- 위 그림의 것들을 컴파일 해줘야한다.
- 코드를 작성하고 모든것이 준비되면 compile 시킨다.
- 위 사진속의 인프라시설들은 안드로이드는 apk IOS는 ipa 다.
- Java와 Xcode로 인프라를 가져와서 apk와 ipa에 넣어준다. 그리고 app store에 보내는것이다.
- 누가 앱을 다운받으면 위 사진의 모든것을 다운받는다.

`npm install --global expo-cli` 다운로드

`Expo go` 다운로드 → 가입 → 컴파일해서 하는것이 아닌 React Native 코드를 바로 폰으로 전송시키는 방식
