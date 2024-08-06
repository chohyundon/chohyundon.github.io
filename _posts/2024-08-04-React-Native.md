---
categories: React Native ProJect
tag: [React Native]
author_profile: false
---

# BottomTab 다양한 속성

![스크린샷 2024-08-03 20 43 17](https://github.com/user-attachments/assets/b609e6ce-b42c-456e-8a0f-1e2c9f1baf7b)

- 현재 개발을 하면서 BottomTab에 속성을 줘야히는 것을 깨달았다..

1. 버튼이 클릭이 되면 색상이 변해야한다.
2. 버튼이 클릭이 되면 SVG도 같이 변해야한다.
3. 헤더 영역을 안보이도록 만들어야 한다.
4. BottomTab의 아이콘들의 이름을 수정해주자.

> 1문제: 해결방법 우선 간단하다. TabBarActiveTintColor를 주면 되었다. 근데 이 속성을 BottomTab의 모든 Screen에 일일히 적용해주는 방법도 있지만, 더 간단한 방법도 있었다. 바로 Navigator에 ScreenOptions에 TabBarActiveTintColor를 적용하면 하위 모든 Screen에 일일히 지정할 필요가 없어진다

<br/>

> 2문제: SVG는 기존의 TabBarIcon의 focused가 되었을 떄 삼항 연산자를 사용히면 되었다.

<br/>

> 3문제: BottomTabs를 만들면서 기본으로 헤더가 나오는것을 볼 수 있는데 이를 방지할 수 있다.headerShown이라는 속성을 통해 이를 관리할 수 있는데, 이 또한 모든 하위 컴포넌트가 안보였으면 좋겠다고
> 생각이 들어 Navigator ScreenOptions에 주었다.

<br />

4.문제: 이 또한 간단하게 title이라는 속성을 통헤서 이름을 각각 지정해주면 끝이다.

```typescript
 <BottomTab.Navigator
      screenOptions={{
        headerShown: false,
        tabBarActiveTintColor: Colors.MoaCarrot.carrot700,
      }}>
      <BottomTab.Screen
        name="Home"
        component={Home}
        options={{
          tabBarIcon: ({focused}) =>
            focused ? <HomeActiveIcon /> : <HomeIcon />,
          title: '홈',
        }}
      />
      <BottomTab.Screen
        name="Coupon"
        component={Coupon}
        options={{
          tabBarIcon: ({focused}) =>
            focused ? <CouponActiveIcon /> : <CouponIcon />,
          title: '쿠폰',
        }}
      />
      <BottomTab.Screen
        name="Share"
        component={Share}
        options={{
          tabBarIcon: ({focused}) =>
            focused ? <ShareActiveIcon /> : <ShareIcon />,
          title: '공유',
        }}
      />
      <BottomTab.Screen
        name="MyPage"
        component={MyPage}
        options={{
          tabBarIcon: ({focused}) =>
            focused ? <MyPageActiveIcon /> : <MyPageIcon />,
          title: 'MY',
        }}
      />
    </BottomTab.Navigator>
```

- 완성된 코드를 볼 수 있다.

### 고민사항

- 코드를 보다 보니 다양한 SVG들을 import해소 사용하고 Component도 사용하다 보니 import가 너무 많아서 이를 어떻게 해결할까 고민이 되었다. 🤔

<br />

- 이를 해결하고자 모듈화를 진행하도록 하였다. 모듈화란 쉽게 말해 현재 컴포넌트로 나누어서 작업하는 것을 말한다. 모듈화를 진행하기 위해 각각 assets 폴더와 screen 폴더에 index 파일을 만들어서 한번에 import 받을수 있도록 관리해봤다.

![스크린샷 2024-08-06 11 50 17](https://github.com/user-attachments/assets/eb4c680b-3fb8-4546-bb33-fcf8ad7dde85)

<br />

- 위 사진처럼 index.ts 파일을 만들었고, 이 파일 안에는 export를 해야하는 컴포넌트들을 모아봤다.

```typescript
export { default as Home } from "./Home";
export { default as Coupon } from "./Coupon";
export { default as Share } from "./Share";
export { default as MyPage } from "./MyPage";
```

- 여기서 default as를 사용한 이유는 별칭을 따로 지정해주고 싶어서 라고 보면 된다.
- 위처럼 index.ts export를 하게 되면

`import {Home, Coupon, Share, MyPage} from '../screen';`

- 위와 같이 어떤 폴더에서 import했는지 한 눈에 알아볼수 있다.

# 터치이벤트 어떤거 사용할까 🤔

- React Native에서는 두 가지의 터치 이벤트 컴포넌트를 제공한다. Pressable과 TouchableOpacity이다.

  <br/>

> 둘의 차이점으로는 TouchableOpacity의 경우 주로 투명도 조절을 위해 사용한다고 한다.Pressable의 경우는 터치상태에 따라 더 다양한 스타일 등을 적용시킬 수 있다고 한다.

<br />
- 결론 : 둘의 차이점은 크지는 않지만 Presable이 좀더 다양한 스타일 적용이 가능해 Pressable을 많이 사용해보자

```typescript
    <Pressable
      onPress={() => navigation.navigate('Login')}
      style={({pressed}) => [
        pressed ? styles.pressed : undefined,
        styles.addButton,
      ]}>
      <Text style={styles.addCouponText}>쿠폰 추가하기</Text>
    </Pressable>
  );
```

- Pressable로 만든 코드이고 style에 pressed라는 속성을 통해 터치가 되었다면, pressed라는 css를 적용시켜 줬다

- onPress를 통해 경로도 같이 이동시키도록 만들었다

# 카카오 로그인 구현하기

- 프로젝트를 만들면서 로그인을 구현해야 하는데 우리는 카카오 로그인 구현을 하기로 하였다.

  > 카카오로그인 참고문서 : https://github.com/crossplatformkorea/react-native-kakao-login?tab=readme-ov-file >

   <br />

- 문서를 보면 우선 React Native Cli의 경우 다음 패키지를 설치해주자
  `yarn add @react-native-seoul/kakao-login`

- 다음으로 `npx-pod install`을 진행해주자

- 이제 ios의 폴더에 있는 info.plist를 수정해줘야 하는데, Xcode로 가서 현재 프로젝트 ios 폴더를 실행 시키고, project의 info로 가서 URL Types를 클릭하고, <strong>URL Schemes에</strong> kakao본인 네이티브키를 넣어주면 끝이다.

- 그 다음 아래 코드를 info.plist에 넣어주면 된다.아마 위 방법을 그대로 실행했다면 cmd + f or Ctrl + f 로 CFBundleVersion를 찾으면 될 것이다

```typescript
+ <key>KAKAO_APP_KEY</key>
+ <string>{카카오 네이티브앱 아이디를 적어주세요}</string>
+ <key>KAKAO_APP_SCHEME</key> // 선택 사항 멀티 플랫폼 앱 구현 시에만 추가하면 됩니다
+ <string>{카카오 앱 스킴을 적어주세요}</string> // 선택 사항
+ <key>LSApplicationQueriesSchemes</key>
+ <array>
+   <string>kakaokompassauth</string>
+   <string>storykompassauth</string>
+   <string>kakaolink</string>
+ </array>
```

- 다음과 같이 수정하면 끝 준비완료다!

```typescript
import { KakaoOAuthToken, login } from "@react-native-seoul/kakao-login";
import { Alert } from "react-native";
import { navigate } from "./NavigationService";

export const signInWithKakao = async (): Promise<void> => {
  const token: KakaoOAuthToken = await login();

  if (token) {
    setTimeout(() => {
      Alert.alert("알림", "로그인 되었습니다", [
        {
          text: "확인",
          onPress: () => navigate("Bottom", { screen: "Home" }),
        },
      ]);
    }, 2000);
  }
};
```

- 햔재 코드를 실행해보니 login이 잘 실행된다!.
- 글로만 보면 모를 수 있으니 참고문서에서 Tutorial에 가서 영상을 보면 더 이해가 잘 될것이다
