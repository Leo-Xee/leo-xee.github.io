---
title: React 프로젝트에 airbnb 컨벤션 적용하기
date: 2021-12-16 17:50:00
category: React
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNails/React.gif)

# 설치하기

## ESLint

ESLint에 airbnb 규칙을 사용하기 위해서는 `eslint-config-airbnb`를 설치해야한다. 하지만 이 패키지가 의존하고 있는 패키지들도 함께 설치해야하기 때문에 다음의 명령어를 사용해서 먼저 의존성 패키지들을 확인한다.

```bash
$ yarn info eslint-config-airbnb peerDependencies
{
  eslint: '^7.32.0 || ^8.2.0',
  'eslint-plugin-import': '^2.25.3',
  'eslint-plugin-jsx-a11y': '^6.5.1',
  'eslint-plugin-react': '^7.27.1',
  'eslint-plugin-react-hooks': '^4.3.0'
}
```

이제 확인한 의존성 패키지들과 함께 `eslint-config-airbnb`를 설치한다.

```bash
yarn add -D eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-react-hooks
```

- `eslint` : 자바스크립트 코드를 검증 및 확인하는 핵심 라이브러리이다.
- `eslint-plugin-import`
  : ES6+의 import/export 문법과 파일 경로 및 파일명이 제대로 입력되었는지를 체크해주는 플러그인이다.
- `eslint-plugin-jsx-a11y` : JSX 엘리먼트의 접근성관련 규칙을 체크해주는 플러그인이다.
- `eslint-plugin-react` : React를 위한 규칙을 추가해주는 플러그인이다.
- `eslint-plugin-react-hooks` : React Hooks를 위한 규칙을 추가해주는 플러그인이다.

## Prettier

**Prettier는 코딩 포맷팅(스타일)을 정리해주는 라이브러리이다.** ESLint 또한 관련 규칙이 있지만 Prettier의 기능이 훨씬 좋기 때문에 보통 ESLint의 포맷팅 관련 규칙들을 꺼주고 함께 사용한다. 이를 위해서 다음과 같이 2가지의 패키지가 더 필요하다.

```bash
yarn add -D prettier eslint-config-prettier eslint-plugin-prettier
```

- `prettier` : 코드 스타일을 정리해주는 라이브러리이다.
- `eslint-config-prettier` : Prettier와 충돌이 생길 수 있는 ESlint의 규칙들을 비활성화한다.
- `eslint-plugin-prettier` : ESlint로 Prettier를 실행하는 플러그인이다.

# 적용하기

## ESLint

프로젝트의 루트 위치에서 `.eslintrc.json` 을 생성하고 다음과 같이 붙여넣는다.

```json
{
  "env": {
    "browser": true,
    "es6": true,
    "node": true
  },
  "extends": ["airbnb", "plugin:prettier/recommended"],
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 2020,
    "sourceType": "module"
  },
  "plugins": ["react", "prettier"]
}
```

## Prettier

프로젝트 루트 위치에 `.prettierrc.json` 을 생성하고 다음과 같이 붙여넣는다.

```json
{
  "singleQuote": false, // 쌍따옴표를 사용
  "semi": true, // 코드 마지막에 세미콜론 사용
  "useTabs": false, // 스페이스바 대신 탭를 사용
  "tabWidth": 2, // 들여쓰기는 2칸
  "trailingComma": "all", // 모든 요소에 쉼표(,)를 붙임
  "printWidth": 80, // 한 줄에 80칸까지만 사용
  "arrowParens": "avoid" // 화살표 함수의 인자가 하나일 경우 괄호 생략
}
```

<br/>
