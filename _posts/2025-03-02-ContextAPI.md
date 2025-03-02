---
layout: single
title: "Context API"
published: true
classes: wide
category: ContextAPI
---


# Context API란

Context API는 React v16부터 사용 가능한 내장 API다.
Props Drilling을 방지하기 위한 개념으로, 앱에 존재하는 컴포넌트들에게 State를 공유하는데 있어 용이하다.

# 라이브러리 대신 Context API를 사용하는 경우

Context API에서 state를 변경하면, Provider로 감싼 모든 자식 컴포넌트가 리렌더링 된다.
그럼 Zustand의 경우, 해당 state를 참조 중인 컴포넌트만 리렌더링 하는데, 왜 Context API를 쓰는가?

Context API는 자주 업데이트할 필요없는 state에 주로 사용한다.
* 테마 상태 (다크 모드, 라이트 모드)
* 사용자 데이터 (인증된 사용자)
* 언어 혹은 지역 데이터

위처럼 간단하고, 전역 상태 저장소를 따로 세팅할 필요가 없을 때 사용하기 좋다.

## 간단한 사용법
react, vite, typescript 환경에서 작성한 코드이다.

> 1. Context 생성
> 2. Provider로 감싸기
> 3. Context 사용

사용하려는 전역 상태를 ```createContext``` 함수를 통해 정의하고,
Provider를 통해 해당 값을 사용할 수 있는 컴포넌트들을 감싸준다.
```javascript
// src/App.tsx
import { createContext } from "react";
import Test from "./Test";

// Context 생성
export const TestContext = createContext("");

function App() {
  return (
    // Provider로 감싸기
    <TestContext.Provider value={"Test"}>
      <Test />
    </TestContext.Provider>
  );
}

export default App;

```

이후 해당 Context를 import 하여 useContext 함수를 통해 사용할 수 있다.

```javascript
// src/Test.tsx
import { useContext } from "react";
import { TestContext } from "./App";

function Test() {
  const value = useContext(TestContext);
  return <div>{value}</div>;
}

export default Test;

```
