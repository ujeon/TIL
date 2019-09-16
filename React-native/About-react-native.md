###리액트 네이티브란?

자바스크립트 기반의 리액트를 사용하여 iOS와 Android 어플리케이션을 만들 수 있도록 도와주는 라이브러리이다.

<br>

###어떻게 시작할까?

기본적으로는 다음과 같은 명령어를 사용하여 새로운 프로젝트를 시작한다.

```
npx react-native init MyFirstReactNative
```

하지만, 모바일 개발이 처음이라면? Expo CLI가 있다. 

먼저 Expo CLI 명령어를 사용하기 위해서 expo-cli를 설치하고,

```
npm install -g expo-cli
```

아래의 명령어를 사용하여 새로운 프로젝트를 시작하면 된다!

```
expo init AwesomeProject

cd AwesomeProject
npm start # you can also use: expo start
```

이렇게 하면 개발 서버가 시작된다.

#### react-native init? expo init?

두 명령어의 차이점은 앞서 말한 것처럼 모바일 개발에 대한 '익숙함'의 차이에 따라 다르게 사용한다. 모바일 개발에 익숙하지 않다면 Expo를 사용하여 손쉽게 시작할 수 있다. Expo는 React Native를 기반으로 만들어진 도구 모음이며 [여러 기능](https://expo.io/features)이 있지만 가장 대표적인 것은 React-Native를 몇 분 안에 만들 수 있다는 점이다. 

모바일 개발이 익숙하다면 React Native CLI를 사용할 수 있다. Expo와는 다르게 Xcode나 Adroid Studio가 필요하다. 

아무래도 처음 개발을 하게되면 이것저것 필요한 도구가 잘 갖춰져 있는 Expo가 편리하고, 내가 필요한 도구를 맞춰서 사용하고 싶다면 React Native이 편리할 것 같다!

<br>

### 리액트 네이티브 앱 실행하기

iOS나 Android 핸드폰에 Expo 앱을 설치하고 컴퓨터와 동일한 무선 네트워크에 연결한다. Android에서는 Expo 앱을 사용하여 터미널에서 QR코드를 스캔하여 프로젝트를 실행하고, iOS에서는 화면 지시에 따라 링크를 얻으면 된다.

#### 앱 수정하기

텍스트 에디터에서 App.js 파일을 수정해보자. 자동으로 변경사항들이 반영된다. 