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
- 위처럼 index.ts export를 하게 되면 `import {Home, Coupon, Share, MyPage} from '../screen';`
- 위와 같이 어떤 폴더에서 import했는지 한 눈에 알아볼수 있다.
