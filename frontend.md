# 스테이지 프론트엔드 스타일 가이드

## 의존성

- 라이브러리를 추가할 때는 신중하게 선택합니다. 손쉽게 구현할 수 있는 기능에 대해서는 의존성을 추가하지 않습니다.

## Linting

- 코딩 스타일에 특별한 선호를 두지 않습니다. 별다른 설정 없이 일관된 코딩 스타일을 유지할 수 있는 방법을 선호하며 특별한 이유가 없다면 [Prettier](https://prettier.io)를 사용합니다. VSCode에서 Prettier extension을 사용합니다.

## 프로젝트 구조

- src
  - components
  - config
    - axios 등 전역 설정이 필요한
  - entities
    - 모델 interface를 정의합니다.
  - hooks
    - 비즈니스 로직을 담고 있는 Hook이 위치합니다.
  - modules
    - redux reducer와 saga가 위치합니다.
  - pages
  - styles
    - global style, theme이 위치합니다.

## 컴포넌트

- Atomic Design을 염두하여 컴포넌트를 설계하되 디렉토리 구조로 atom, molecule, organism을 구별하지 않습니다.
- 필요한 경우 어떤 타입의 컴포넌트인지 주석으로 남길 수 있습니다.
- 컴포넌트 구조는 가급적 Nesting level이 깊지 않도록 합니다. 컴포넌트가 3단계 이상 nesting된다면 별도의 컴포넌트를 만들어야 하는 타이밍 일 수 있습니다.
- 컴포넌트에 비즈니스 로직이 들어가지 않도록 합니다. 비즈니스 로직은 Hook을 통해 분리합니다. `node_modules`에 있는 모듈 의존성이 컴포넌트에 들어간다면 다시 한번 생각해봐야 합니다.

## Hook

- 가급적 다양한 훅을 만듭니다.
- 1개의 파일에서 1개의 function을 export 합니다. 이 규칙은 검색을 편하게 해줍니다.

## Redux

- 꼭 필요한 경우만 사용합니다.
- 전역 상태로 관리했을 때 구조가 더 편리해지는 경우 사용합니다. 로그인 세션 관리가 대표적인 예시입니다.
- Redux는 보일러플레이트 코드가 많으므로 [redux-toolkit](https://redux-toolkit.js.org)을 사용합니다.
- 네트워크 요청과 같은 부수 효과는 [redux-saga](https://redux-saga.js.org)를 사용하여 처리합니다. 단순한 get request의 경우는 구현이 오히려 복잡해지므로 redux를 사용하지 않고 [swr](http://swr.vercel.app)을 사용합니다.

## 모듈 경로

- 우리가 작성하는 모듈의 `import`는 `@` 접두사를 사용합니다. 예시는 다음과 같습니다.

```javascript
import useSWR from "swr"; // 외부 모듈
import useMyHook from "@/hooks/useMyHook"; // 우리 모듈
```

- 모듈 경로는 `tsconfig.json`의 설정을 이용합니다.

```json
{
  "compilerOptions": {
    ...,
    "baseUrl": "node_modules",
    "paths": { "@/*", ["../src/*"] }
  }

}
```
