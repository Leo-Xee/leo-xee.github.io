---
title: 밑바닥부터 React 개발 환경 구축하기 - 4. Jest와 Testing-Library
date: 2022-04-11 11:03:76
category: React
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNails/React.gif)

이 글에서는 밑바닥부터 개발 환경 구축하기 3편에 이어서 Jest와 react-testing-library를 사용해서 테스트 환경을 구축하는 과정을 정리합니다.

# 시리즈

- [밑바닥부터 React 개발 환경 구축하기 - 1. Webpack](https://leo-xee.github.io/React/react-setup-webpack/)
- [밑바닥부터 React 개발 환경 구축하기 - 2. Typescript](https://leo-xee.github.io/React/react-setup-ts/)
- [밑바닥부터 React 개발 환경 구축하기 - 3. Eslint와 Prettier](https://leo-xee.github.io/React/react-setup-eslint/)
- 밑바닥부터 React 개발 환경 구축하기 - 4. Jest와 Testing-Library

# 설치

Jest와 react-testing-library를 위해서 패키지를 설치한다.

```bash
# Jest 관련 패키지들
$ yarn add -D jest @types/jest ts-jest jest-environment-jsdom ts-node

# react-testing-library 관련 패키지들
$ yarn add -D @testing-library/jest-dom @testing-library/react @testing-library/user-event

# Jest를 위한 ESLint 플러그인
$ yarn add -D eslint-plugin-jest
```

- `jest` : Node.js기반 테스트 프레임워크
- `@types/jest` : Jest를 위한 타입 패키지
- `ts-jest` : TS 파일을 JS로 변환해주는 패키지

  (Babel7부터는 타입스크립트를 지원하기 때문에 `babel-jest`를 사용해도되지만 [여기](https://kulshekhar.github.io/ts-jest/docs/babel7-or-ts)에 나와 있는 한계점을 참고해보고 타입 체크를 생각해서 `ts-jest`를 사용했다. )

- `jest-environment-jsdom` : Jest를 실행할 가상 DOM 패키지

  (Jest 28버전부터 Jest와 분리되어서 jsdom을 사용하기 위해서는 설치가 필요하다.)

- `ts-node` : Node.js 상에서 타입스크립트를 컴파일해주는 패키지
  (타입스크립트로 config를 작성하기 위해서 필요하다.)
- `@testing-library/jest-dom` : Jest의 DOM에 특화된 matcher 패키지
- `@testing-library/react` : 코어인 `@testing-library/dom`을 React에 맞게 포팅한 패키지
- `@testing-library/user-event` : 실제로 유저 이벤트를 발생시켜주는 패키지
- `eslint-plugin-jest` : Jest를 위한 ESLint 규칙을 포함하고 있는 플러그인 패키지

# 설정

## @testing-library

프로젝트의 루트 위치에 `setupTests.ts` 파일을 생성하고 다음과 같이 작성한다.

```tsx
/* setupTests.ts */

/*
 * 각 테스트 파일에서 @testing-library를 사용하기 위해 필요한 코어를 import한다.
 * 이는 Jest의 config파일에서 사용되며 이를 통해 import의 반복을 줄일 수 있다.
 */

import '@testing-library/jest-dom/extend-expect'
```

## Jest

프로젝트의 루트 위치에 `jest-config.ts` 파일을 생성하고 다음과 같이 작성한다.

( 참고: [ts-jest](https://kulshekhar.github.io/ts-jest/docs/getting-started/installation), [jest](https://jestjs.io/docs/configuration) )

```tsx
/* jest.config.ts */

import type { InitialOptionsTsJest } from "ts-jest";

const config: InitialOptionsTsJest = {
  preset: "ts-jest",  // .ts와 .tsx을 CommonJS로 변환
  testEnvironment: "jsdom",  // 테스트 환경으로 jsdom을 사용
  setupFilesAfterEnv: ["<rootDir>/setupTests.ts"],  // 각각의 테스트 전에 실행할 모듈을 경로
  moduleNameMapper: {
    "^@/(.*)$": "<rootDir>/src/$1",  // tsconfig에서 절대경로 사용 시에 jest가 인식하도록 경로 매핑
  },
};

export default config;
```

## ESLint

ESLint에 Jest를 위한 설정을 다음과 같이 작성한다.

```json
/* .eslintrc.json */
{
  // ...
  "env": {
    "browser": true,
    "es6": true,
    "node": true,
    "jest": true // 추가
  },
  // ...
  "ignorePatterns": ["webpack.config.js", "jest.config.ts", "setupTests.ts"], // 추가
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "plugin:prettier/recommended",
    "plugin:jest/recommended" // 추가
  ],
  "plugins": ["prettier", "@typescript-eslint", "jest"] // 추가
  // ...
}
```

`package.json`에 테스트를 실행하기 위한 scripts를 다음과 같이 작성한다.

```json
/* package.json */

{
  // ...
  "scripts": {
    "dev": "webpack serve",
    "build": "webpack --config webpack.config.js --mode production",
    "test": "jest --watchAll" // 추가
  }
  // ...
}
```

# 참조

- [https://jestjs.io/docs/tutorial-react](https://jestjs.io/docs/tutorial-react)
- [https://testing-library.com/docs/react-testing-library/setup](https://testing-library.com/docs/react-testing-library/setup)
- [https://kulshekhar.github.io/ts-jest/](https://kulshekhar.github.io/ts-jest/)
- [https://www.youtube.com/watch?v=7uKVFD_VMT8&t=530s](https://www.youtube.com/watch?v=7uKVFD_VMT8&t=530s)
- [https://stackoverflow.com/questions/69227566/consider-using-the-jsdom-test-environment](https://stackoverflow.com/questions/69227566/consider-using-the-jsdom-test-environment)

<br>
