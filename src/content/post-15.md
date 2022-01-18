---
title: "Frontend Architecture with React-1"
date: "2021-11-11"
draft: true
path: "/blog/frontendArchitecture"
---

네스트로 서버를 구축하고나서 잘 짜여진 아키텍쳐가 개발자에게 얼마나 큰 효용을 줄 수 있는지를 너무나도 크게 느꼈다.
개인 프로젝트인 choichoikule studio e-commerce site를 구축하면서 프론트엔드도 그러한 잘 짜여진 아키텍쳐 위에서
작업하면 좋겠다는 생각을 하게 되었다. [Bulletproof React](https://github.com/alan2207/bulletproof-react)
프로젝트 base에서 내가 원하는 방향으로 조금 수정한 아키텍쳐에 대한 설명을 시리즈로 포스팅할 예정이다.

아키텍쳐에서 중요한 것은 관심사의 분리, 일관된 컨벤션, 확장성이다.
구성요소가 많아지면서 장황해지는 것을 최대한 경계한 다.

## 아키텍쳐 구성요소

컴포넌트를 정리하는 방식
-> atomic design (atom>molecule>organism>template>page)
atom: button과 같은 작은 단위의 컴포넌트
api로 부터 수신한 데이터는 customhook으로 분리한다.
내가 받을 리스트는 useProductList, useProductDetail, useUser 등등이 될 수 있겠다.

plop을 사용해서 필요한 기본 조합 제너레이트
웹 바이탈체크,
테스트 포함,
리액트 18의 suspense 포함 렌더링직후 불러오기의 워터폴문제, 혹은 중앙화방식에서 모두를 기다리는것 방지가능 데이터가 계속 흘러들어옴에 따라, React는 렌더링을 다시 시도하며, 그 때마다 React가 “더 깊은 곳까지” 처리할 수 있게 될 겁니다.
zustand 상태관리 라이브러리/ 서버스테이트를 다루는 리액트 쿼리
서비스워커란 무엇인가? 서비스 워커를 사용하면 오프라인인 경우에도 일관적인 경험을 제공한다음 네트워크 연결이 돌아올때 더 많은 데이터를 불러오게 할 수 있다. 백그라운드 동기화, 푸시알람의 기능을 수행할 수 있다.
네이티브앱에서는 이미 널리 구현되는 기법이다. 제일 나중의 문제로 남겨둔다.

리액트 헬멧과 개츠비로 메타데이터 변경

# 상태값을 관리하기

리액트에서 상태를 관리하기 위한 수 많은 방법이 존재한다.
이 프로젝트에서는 zustand, react-query, suspense, custom hook, react-query-auth을 사용할 것 이다.

#### 기본적인 상태 (zustand)

아래와 같이 store를 정의하고 가져와 사용하면 된다.

```javascript
import create from "zustand"

// set 함수를 통해서만 상태를 변경할 수 있다
const useStore = create((set) => ({
  bears: 0,
  increasePopulation: () => set((state) => ({ bears: state.bears + 1 })),
  removeAllBears: () => set({ bears: 0 }),
}))

// 상태를 꺼낸다
function BearCounter() {
  const bears = useStore((state) => state.bears)
  return <h1>{bears} around here ...</h1>
}

// 상태를 변경하는 액션을 꺼낸다
function Controls() {
  const increasePopulation = useStore((state) => state.increasePopulation)
  return <button onClick={increasePopulation}>one up</button>
}
```

#### 유저에관한 상태

리액트쿼리오쓰를 사용한다.

```javascript
// src/lib/auth.ts

import { initReactQueryAuth } from 'react-query-auth';
import { loginUser, loginFn, registerFn, logoutFn } from '...';

interface User {
  id: string;
  name: string;
}

interface Error {
  message: string;
}

const authConfig = {
  loadUser,
  loginFn,
  registerFn,
  logoutFn,
};

export const { AuthProvider, useAuth } = initReactQueryAuth<
  User,
  Error,
  LoginCredentials,
  RegisterCredentials
>(authConfig);
```

```javascript
import { QueryClient } from 'react-query';
import { QueryClientProvider } from 'react-query/devtools';
import { AuthProvider } from 'src/lib/auth';

const queryClient = new QueryClient();

export const App = () => {
  return (
    <QueryClientProvider client={queryClient}>
      <AuthProvider>
        {//the rest the app goes here}
      </AuthProvider>
    </QueryClientProvider>
  );
};
```

```javascript
import { useAuth } from "src/lib/auth"

export const UserInfo = () => {
  const { user } = useAuth()
  return <div>My Name is {user.name}</div>
}
```

- 서버상태
- 데이터결과
- 데이터 상관없는 ui 동작제어를 위한 상태

참고:  
[zustand](https://ui.toast.com/weekly-pick/ko_20210812)
