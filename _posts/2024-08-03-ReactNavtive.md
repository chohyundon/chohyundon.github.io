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

  `{"Group": [Function Group], "Navigator": [Function BottomTabNavigator], "Screen": [Function Screen]}`

<br>
