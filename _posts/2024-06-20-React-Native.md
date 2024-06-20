---
categories: React Native Study
tag: [React Native]
author_profile: false
---

> 몸이 갑자기 아파져서 공부를 못하게 되었다...
> 이번에 GDSC에서 새로운 프로젝트를 하게 되었는데 바로 React Native를 공부해야한다!!

# Expo?

- React Native를 설치하기 위해서는 expo를 많이 사용한다고 한다 이유가 뭘까?
- 우선 간단하게 Expo는 React Native를 javascript를 사용해서 모바일 앱을 빌드하는 프레임 워크라고 한다.
- 또 한 Expo는 facebook에서 개발했다고 한다.
- expo를 사용하는 이유는 간단하게 시뮬레이터 없이도 웹에서 결과를 볼 수 있다는 점이 있다.

<br>

# 프로젝트 세팅

- expo 사이트에 접속하기 여기 사이트로 들어가서 명령어를 입력하자

- 사이트 : [Create a project](https://docs.expo.dev/get-started/create-a-project/)

```html
npx create-expo-app@latest
```

- 하지만 강의에서는 조금 다르게 접근했다…. 그래서 어떤 것이 맞는지 .. 몰라서 1시간이나 할애했다..

```html
npm install -g expo-cli
```

- 다음과 같이 작성을 하고

```html
expo init RNCourse
```

- 다음 명령어를 입력해주면 다음과 같이 3가지 항목이 나타나는데 나는 2번째 타입스크립트를 사용하고 빈 프로잭트를 생성했다

<img width="750" alt="Rn" src="https://github.com/chohyundon/web-pracitce/assets/113508075/51e6ce19-e6ff-42ff-a08a-051144f05f1c">

- npx create-expo-app@latest의 명령어와 npm install -g expo-cli 명령어를 입력하면 살짝 폴더 구조가 다르다. 그래서 편한대로 사용하면 될거 같다…

<br>

# React Native 만의 컴포넌트

- React Native에서 기존의 div or h1 h2 태그등은 View Text로 변환해서 사용해야 한다.
- View 컴포넌트 안에는 텍스트를 삽입할 수 없다 ex) <View>hi</View> ❌
- View 컴포넌트는 다른 컴포넌트를 담고 배치하는 컴포넌트이다!!
- 텍스트를 작성하고 싶으면 Text컴포넌트를 꼭 활용해서 작성하자

```typescript
import { StatusBar } from "expo-status-bar";
import { StyleSheet, Text, View } from "react-native";

export default function App() {
  return (
    <View style={styles.container}>
      <Text>Hello World</Text>
      <StatusBar style="auto" />
    </View>
  );
}
```

- 이 외에도 다양한 컴포넌트들이 있다. ex)Button, TextInput …. 이거는 사용해보면서 익숙해지도록 노력해봐야겠다!!

<br>

# React Native css

- React Native에서 css를 작성할 때 stylesheet 객체를 사용해서 만들어야 한다
- 사용해보니 styleX와 아주 유사한 점이 많았다.

```typescript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center",
  },
});
```

- styleX 처럼 StyleSheet을 create하고 만들면 되는 것 같다
- React Native의 css는 기존 px 대신 그냥 숫자만 입력하면 알아서 px 처럼 적용이 된다. ex) margin : 16

<br>

# React Native 이벤트 핸들러 처리

- React Native 에서 이벤트 핸들러를 처리할 때는 react와는 크게 다르지 않다.
- React에서 input 값의 변경 상태를 확인해주는 onChange 핸들러의 이름을 onChangeText로 변경해주면 끝이었다.
- 다만 onChange와 onChangeText는 차이가 존재하는데 onChange의 경우 event 객체로 다양한 정보를 가져올수 있지만 onChangeText의 경우 오로지 변경된 텍스트 자체 값만 직접 인자로 받는다.
- 또 한 버튼의 onClick 대신 onPress도 처리하면 되었다

```typescript
import { useState } from "react";
import { Button, StyleSheet, Text, TextInput, View } from "react-native";

export default function App() {
  const [enteredText, setEnteredText] = useState("");

  function goalInputHandler(enteredText: any) {
    setEnteredText(enteredText);
  }

  function addGoalHandler() {
    console.log(enteredText);
  }

  return (
    <View style={styles.addContainer}>
      <View style={styles.inputContainer}>
        <TextInput
          placeholder="Your course goal!"
          style={styles.textInput}
          onChangeText={goalInputHandler}
        />
        <Button title="ADD GOAL" onPress={addGoalHandler} />
      </View>
      <View style={styles.goalContainer}>
        <Text>List of goals...</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  addContainer: {
    flex: 1,
    paddingTop: 50,
    paddingHorizontal: 16,
  },
  inputContainer: {
    flex: 1,
    flexDirection: "row",
    justifyContent: "space-between",
    alignItems: "center",
    borderBottomWidth: 1,
    marginBottom: 24,
    borderColor: "#cccccc",
  },
  textInput: {
    borderWidth: 1,
    borderColor: "#cccccc",
    width: "70%",
    marginRight: 8,
    padding: 8,
  },
  goalContainer: {
    flex: 5,
  },
});
```

<br>

# React Native 에서 android vs ios 스타일링 차이

- React Native를 공부하면서 안드로이드에는 borderRadius를 Text에 style을 줘도 잘 적용됬지만 ios는 View를 생성하고 View에 스타일링 줘야 적용이 됬다… 이런 스타일링등이 몇몇 차이가 있어서 주의해보자!!

<br>

# React Native 에서 Scroll을 구현해보기

- React Native에서는 ScrollView라는 독자적인 컴포넌트를 Native에서 지원을 해준다 이를 활용하면 쉽게 구현이 가능하다

```typescript
import { useState } from "react";
import {
  Button,
  StyleSheet,
  Text,
  TextInput,
  View,
  ScrollView,
} from "react-native";

export default function App() {
  const [enteredText, setEnteredText] = useState("");
  const [courseGoals, setCourseGoals] = useState<string[]>([]);

  function goalInputHandler(enteredText: string) {
    setEnteredText(enteredText);
  }

  function addGoalHandler() {
    setCourseGoals((currentValue: string[]): string[] => [
      ...currentValue,
      enteredText,
    ]);
  }

  return (
    <View style={styles.addContainer}>
      <View style={styles.inputContainer}>
        <TextInput
          placeholder="Your course goal!"
          style={styles.textInput}
          onChangeText={goalInputHandler}
        />
        <Button title="ADD GOAL" onPress={addGoalHandler} />
      </View>
      <View style={styles.goalContainer}>
        <ScrollView alwaysBounceVertical={false}>
          {courseGoals.map((goal, index) => (
            <View key={index} style={styles.goalItem}>
              <Text style={styles.goalText}>{goal}</Text>
            </View>
          ))}
        </ScrollView>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  addContainer: {
    flex: 1,
    paddingTop: 50,
    paddingHorizontal: 16,
  },
  inputContainer: {
    flex: 1,
    flexDirection: "row",
    justifyContent: "space-between",
    alignItems: "center",
    borderBottomWidth: 1,
    marginBottom: 24,
    borderColor: "#cccccc",
  },
  textInput: {
    borderWidth: 1,
    borderColor: "#cccccc",
    width: "70%",
    marginRight: 8,
    padding: 8,
  },
  goalContainer: {
    flex: 5,
  },
  goalItem: {
    margin: 8,
    padding: 8,
    borderRadius: 6,
    backgroundColor: "#5e0acc",
  },
  goalText: {
    color: "white",
  },
});
```

- ScrollView 속성에 alwaysBounceVertical={false} 이 거는 ios에서만 사용이 가능하고 효과는 스크롤할 컨텐츠가 없다면 튀어오르는 효과 같은거를 제거 해주는 속성이다. 이 외에도 다양한 속성을 공식문서를 통해 읽어봐야겠다

참고 : <a href="https://reactnative.dev/docs/scrollview">https://reactnative.dev/docs/scrollview</a>

<br>

# ScrollView 개선

- ScrollView의 한가지 문제점이 있다.
- 바로 ScrollView는 항목이 몇개든 상관없이 모든 자식 항목을 렌더링하는 문제가 생긴다 즉 렌더링 갯수가 많아지면 렌더링에 문제가 생긴다.. 😭
- 이를 개선하기 위해서 FaltList를 사용해보자

```typescript
import { useState } from "react";
import {
  Button,
  StyleSheet,
  Text,
  TextInput,
  View,
  FlatList,
} from "react-native";

interface Goal {
  text: string;
  id: string;
}

export default function App() {
  const [enteredText, setEnteredText] = useState("");
  const [courseGoals, setCourseGoals] = useState<Goal[]>([]);

  function goalInputHandler(enteredText: string) {
    setEnteredText(enteredText);
  }

  function addGoalHandler() {
    setCourseGoals((currentValue) => [
      ...currentValue,
      { text: enteredText, id: Math.random().toString() },
    ]);
  }

  return (
    <View style={styles.addContainer}>
      <View style={styles.inputContainer}>
        <TextInput
          placeholder="Your course goal!"
          style={styles.textInput}
          onChangeText={goalInputHandler}
        />
        <Button title="ADD GOAL" onPress={addGoalHandler} />
      </View>
      <View style={styles.goalContainer}>
        <FlatList
          data={courseGoals}
          renderItem={(itemData) => {
            return (
              <View style={styles.goalItem}>
                <Text style={styles.goalText}>{itemData.item.text}</Text>
              </View>
            );
          }}
          keyExtractor={(item) => {
            return item.id;
          }}
          alwaysBounceVertical={false}
        />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  addContainer: {
    flex: 1,
    paddingTop: 50,
    paddingHorizontal: 16,
  },
  inputContainer: {
    flex: 1,
    flexDirection: "row",
    justifyContent: "space-between",
    alignItems: "center",
    borderBottomWidth: 1,
    marginBottom: 24,
    borderColor: "#cccccc",
  },
  textInput: {
    borderWidth: 1,
    borderColor: "#cccccc",
    width: "70%",
    marginRight: 8,
    padding: 8,
  },
  goalContainer: {
    flex: 5,
  },
  goalItem: {
    margin: 8,
    padding: 8,
    borderRadius: 6,
    backgroundColor: "#5e0acc",
  },
  goalText: {
    color: "white",
  },
});
```

- FlatList를 사용할 때는 data + renderItem + keyExtractor가 필요하다. 여기서 Data는 리스트의 source를 담는 Props이고, renderItem은 수많은 item을 렌더링 시켜주는 콜백함수이다. keyExtractor는 고유의 키 값을 주기 위해서 사용하는 props이다.
  <br>
- 여기서 key 값을 무작위로 줄 때는 addGoalHandler의 함수처럼 객체로 나누어서 주도록 노력해보자
