---
categories: React Native ProJect
tag: [React Native]
author_profile: false
---

# MyPage 작업하기

<img width="376" alt="스크린샷 2024-08-08 13 36 40" src="https://github.com/user-attachments/assets/3e4f2147-a068-4090-a979-71f3c813dd4d">

- 우선 MyPage를 작업하기 전에 가장 눈에 띄는것은 유저 프로필이다.

> 유저 프로필을 가져오는 방법은 카카오톡 로그인을 구현을 완료하고, 정보를 가져오는 함수를 호출하면 된다.

- 로그인 구현 함수

```typescript
import { KakaoOAuthToken, login } from "@react-native-seoul/kakao-login";

const signInWithKakao = async (): Promise<void> => {
  const token: KakaoOAuthToken = await login();
};
```

- 위와 같이 만들면 카카오 로그인을 할 수 있는 카카오 로그인 창이 나오는것을 볼 수 있다.

<br>

- 유저 정보 가져오는 함수

```typescript
import { getProfile, KakaoProfile } from "@react-native-seoul/kakao-login";

const KakaoUserData = async (): Promise<void> => {
  const profile: KakaoProfile = await getProfile();
};
```

- 위와 같이 함수를 만들게 된다면 로그인이 되었을 때 getProfile 함수를 호출하고 콘솔에 profile을 찍어보면 결과값이 잘 나올것이다. 나의 경우 필요한 데이터 nickname + profileUrl를 추출했다.

## 🤔 새로고침하면 데이터가 사라진다...

- 위 데이터를 활용해 MyPage의 대이터를 넣었고, 새로고침이 될 시 유저 정보가 사라져버리는 것을 볼 수 있었다...
- 그래서 해결책을 찾아보니 AsyncStorage에 데이터를 저장해야한다는 것을 알게 되었다.

- [공식문서] : https://react-native-async-storage.github.io/async-storage/docs/usage/

- 공식문서를 찾아보니 setItem으로 데이터를 저장하고, getItem으로 저장한 데이터를 불러올 수 있었다.

```typescript
import { getProfile, KakaoProfile } from "@react-native-seoul/kakao-login";
import AsyncStorage from "@react-native-async-storage/async-storage";
import { UserDataProp } from "../type/type";

export const KakaoUserData = async (): Promise<void> => {
  try {
    const profile: KakaoProfile = await getProfile();

    const userData: UserDataProp = {
      name: profile?.nickname,
      img: profile?.profileImageUrl,
    };

    if (profile) {
      await AsyncStorage.setItem("user", JSON.stringify(userData));
    }
  } catch (e) {
    console.error(e);
  }
};
```

- 위 코드를 작성하면서 setItem의 value 2번째 인자의 값은 string 문자열이어야 한다는 것이었다.
- 그래서 userData를 JSON.stringify 통해 문자열로 변환해서 값을 넣어줬다.

- 데이터를 가져올 떄는 JSON.pares를 통해 getItem 대이터를 가져오면 된다.

# 전역상태관리 Jotai

- 코드를 만들다 보니 전역상태관리가 필요해졌다..
- user의 정보가 전역적으로 사용을 해야하다보니 jotai가 생각이 나서 만들어봤다.

- 조타이 [공식문서] : https://jotai.org/
- 우선 간단하게 atom이라는 폴더를 생성했고 user의 atom을 생성해주었다.

```typescript
import { atom } from "jotai";
import { UserDataProp } from "../type/type";

export const userAtom = atom<UserDataProp | null>(null);
```

- 간단하게 코드를 설명하자면 userAtom에 기본값 초기값을 설정해주었다.
- 초기값을 설정해주었으니, 아래와 같이 useAtom을 사용해서 현재 setUserData의 값을 업데이트 해주었으니 값을 읽고 수정하기 수월해졌다.

```typescript
const [userData, setUserData] = useAtom<UserDataProp | null>(userAtom);

useEffect(() => {
  const user = async () => {
    const data = await getUser();
    setUserData(data);
  };

  user();
}, [setUserData]);
```

- 다른 컴포넌트에서 userData을 사용해보고 싶으면 const [userData] = useAtom(userAtom) 이렇게만 간단하게 작성하면 데이터를 전역적으로 사용이 가능해진다!!
