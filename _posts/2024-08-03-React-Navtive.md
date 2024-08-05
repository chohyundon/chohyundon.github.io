---
categories: React Native ProJect
tag: [React Native]
author_profile: false
---

# 프로젝트 개요

- 기프티콘을 관리하는 어플을 만드는 것에 흥미가 있어서 프로젝트에 참여하게 되었다 😀

# 메인 페이지 만들기

![스크린샷 2024-08-03 20 43 17](https://github.com/user-attachments/assets/b609e6ce-b42c-456e-8a0f-1e2c9f1baf7b)

- 메인 페이지를 봤을 때 가장 눈에 띄는 하단 BottomTabs부터 만들어 보자

## BottomTabs

1. 패키지 설치
   `npm install @react-navigation/bottom-tabs`
   (공식문서 - https://reactnavigation.org/docs/bottom-tab-navigator)

<br />

2. 사용법

- createBottomTabNavigator를 import를 먼저 해주자
- 다음에 import한 createBottomTabNavigator를 호출해주자

  -> Tab의 값을 콘솔에 찍어서 확인해보니

  ` {"Group": [Function Group], "Navigator": [Function BottomTabNavigator], "Screen": [Function Screen]}`

<br />

> 간단하게 <strong>Group</strong>은 여러화면에 공통된 속성을 관리할 떄 유용할거 같다.
> 아래 코드 처럼 Group으로 Home, Profile을 같은 속성을 사용할 때 다음과 같이 사용하면 유용할거 같다

> Navigator는 사용자가 버튼을 클릭할 때 화면 전환을 할 수 있도록 도와준다고 한다. 그래서 보통 가장 먼저 Navigator를 선언한다.

> 마지막으로 Screen은 화면 구성을 나타내는데 component에 버튼으로 화면 전환 했을 때 렌더링이 되도록 만든다

```javascript
<Tab.Navigator>
  <Tab.Group
    screenOptions={{
      headerShown: false,
      tabBarStyle: { backgroundColor: "blue" },
    }}
  >
    <Tab.Screen name="Home" component={HomeScreen} />
    <Tab.Screen name="Profile" component={ProfileScreen} />
  </Tab.Group>
  <Tab.Screen name="Settings" component={SettingsScreen} />
</Tab.Navigator>
```

<br />

```javascript
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";

const Tab = createBottomTabNavigator();

function MyTabs() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );
}
```
