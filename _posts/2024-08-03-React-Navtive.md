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
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import HomeScreen from "./homeScreen";
import SettingsScreen from "./settingsScreen";

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

- 위 코드를 간단히 설명하자면 Tab 변수에 createBottomTabNavigator 함수를 호출해 객체를 변수에 담는다. createBottomTabNavigator함수는 위의 콘솔에 나온 객체를 가지고 있는 함수다

> Tab.Navigator는 위의 설명처럼 BottomTab을 사용할 때 화면전환을 도와준다.
> Tab.Screen은 버튼을 눌러 화면을 전환했을 때 나오는 페이지를 화면에 보여준다. 그래서 component={버튼을 누를 때 랜더링 되어야 하는 컴포넌트}를 import 해주자

## SVG 사용하기

- 기존의 react를 통해 개발할 때는 기본적으로 SVG 파일을 import해서 사용이 가능했지만, react native는 따로 패키지를 설치해줘야 한다.

#### 🤔 여기서 고민이 생겼다. SVG를 사용하는 밥법이 Expo와 React Native Cli가 서로 다른 방법을 택하고 있어서 Expo vs React Native Cli를 고민해봤다.

<br />

- React native를 개발할 때 두 가지 방법이 있는데 Expo와 React Native Cli가 있다.
  > 이 둘의 차이점은
  > 우선 <strong>Expo</strong>의 경우 React Native를 처음 접한 사람들에게 보다 쉬운 개발을 할 수 있다. Expo 자체에서 많은 라이브라리와 문서등을 지원해줘서 개발하기는 쉬운거 같다. <strong>하지만</strong> 문제점은 Expo에서 지원해주는 라이브러리만을 사용해야 해서 Expo에 없는 라이브러리는 사용을 못한다.
  > <strong>React Native Cli</strong>의 경우 처음 진입 장벽은 Expo보다는 높지만 Expo보다 더 다양한 라이브러리와 빌드할 때 더욱 빠르다 라는 장점이 있다.

#### SVG 사용해보기

- <strong>Expo</strong> : https://docs.expo.dev/ui-programming/using-svgs/
- Expo에서 시용법은 아래 명령어를 통해 패키지를 설치해주자
  `npx expo install react-native-svg`
- 다음으로는 SVG파일 자체를 그대로 아래 코드처럼 가져와서 사용해야한다. 여기서 <strong>⚠️주의사항</strong>은 SVG 파일은 첫번째가 모두 대문자로 시작해야 한다.

```javascript
import React from "react";
import { Text, View, StyleSheet } from "react-native";
import Svg, { Path } from "react-native-svg";

export default function TriangleDown() {
  return (
    <View style={styles.container}>
      <Svg width={20} height={20} viewBox="0 0 20 20">
        <Path d="M16.993 6.667H3.227l6.883 6.883 6.883-6.883z" fill="#000" />
      </Svg>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
  },
});
```

- 위 방법의 단점은 코드의 가독성이 많이 떨어진다. 기존의 React는 SVG파일의 경로를 import해서 사용했었는데...

- <strong>React Native CLi</strong> : https://www.npmjs.com/package/react-native-svg#installation
- 우선 나는 yarn을 사용해볼거기 때문에 `yarn add react-native-svg`을 사용한다. 다음으로 `cd ios && pod install` 을 통해 라이브러리를 설치 해준다

- 추가사항으로 React Native CLi는 자체적으로 SVG파일 자체를 Import를 할 수가 없어서 `react-native-svg-transformer`를 설치해서 import 할 수 있도록 만들어보자

- 다음으로 `metro.config.js` 파일을 수정해줘야 한다. Native 버전이 0.72이상이면

```javascript
const { getDefaultConfig, mergeConfig } = require("@react-native/metro-config");

const defaultConfig = getDefaultConfig(__dirname);
const { assetExts, sourceExts } = defaultConfig.resolver;

/**
 * Metro configuration
 * https://facebook.github.io/metro/docs/configuration
 *
 * @type {import('metro-config').MetroConfig}
 */
const config = {
  transformer: {
    babelTransformerPath: require.resolve("react-native-svg-transformer"),
  },
  resolver: {
    assetExts: assetExts.filter((ext) => ext !== "svg"),
    sourceExts: [...sourceExts, "svg"],
  },
};

module.exports = mergeConfig(defaultConfig, config);
```

- 이하 버전은 아래와 같이 파일을 수정해주자

```javascript
const { getDefaultConfig } = require("metro-config");

module.exports = (async () => {
  const {
    resolver: { sourceExts, assetExts },
  } = await getDefaultConfig();
  return {
    transformer: {
      babelTransformerPath: require.resolve("react-native-svg-transformer"),
    },
    resolver: {
      assetExts: assetExts.filter((ext) => ext !== "svg"),
      sourceExts: [...sourceExts, "svg"],
    },
  };
})();
```

- 그리고 추가적으로 타입스크립트를 사용중이라면, global.d.ts 파일을 생성해주고(파일 이름은 다른 이름 대체가능)

```typescript
declare module "*.svg" {
  import React from "react";
  import { SvgProps } from "react-native-svg";
  const content: React.FC<SvgProps>;
  export default content;
}
```

- 세팅은 마무리가 되었고, 사용을 해보자

```typescript
import React from 'react';
import {SafeAreaView, Text} from 'react-native';
import Icon from './assets/Vector.svg';

export default function App() {
  return (
    <SafeAreaView>
      <Text>hi</Text>
      <Icon width={120} height={40} fill={'#FFF'} />
    </SafeAreaView>
  );
}

```

![스크린샷 2024-08-05 18 09 48](https://github.com/user-attachments/assets/34d16937-b412-48f4-add8-edd6f4e87760)

- 화면을 보니 잘 나오는것을 볼 수 있다 !!!😀
