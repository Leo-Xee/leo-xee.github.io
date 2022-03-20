---
title: 밑바닥부터 React 개발 환경 구축하기 - 3. Eslint와 Prettier
date: 2022-02-05 12:55:00
category: React
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNails/React.gif)

이 글에서는 밑바닥부터 React 개발 환경 구축하기 2편에 이어서 eslint와 prettier 그리고 airbnb의 typescript lint 규칙을 설정하는 과정을 정리합니다.

# 시리즈

- [밑바닥부터 React 개발 환경 구축하기 - 1. Webpack](https://leo-xee.github.io/React/react-setup-webpack/)
- [밑바닥부터 React 개발 환경 구축하기 - 2. Typescript](https://leo-xee.github.io/React/react-setup-ts/)
- 밑바닥부터 React 개발 환경 구축하기 - 3. Eslint와 Prettier

# eslint, prettier 관련 패키지 설치

eslint와 prettier 그리고 typescript의 eslint 관련 패키지들을 설치한다.

```bash
# eslint와 prettier
$ yarn add -D eslint prettier

# eslint의 formatter을 off하고 prettier를 사용하기 위한 패키지들
$ yarn add -D eslint-config-prettier eslint-plugin-prettier

# typescript를 lint하기 위한 패키지들
$ yarn add -D @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

Eslint에 airbnb 규칙을 적용하기 위해서 `eslint-config-airbnb`가 다음과 같이 의존하고 있는 패키지들을 먼저 확인한다.

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

이제 확인한 의존성 패키지들과 함께 다음의 패키지들을 모두 설치한다.

```bash
# airbnb 규칙
$ yarn add -D eslint-config-airbnb

# airbnb 규칙의 의존성 패키지들
$ yarn add -D eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-react-hooks

# airbnb 타입스크립트 규칙
$ yarn add -D eslint-config-airbnb-typescript
```

# 설정

프로젝트의 루트 위치에 `.eslinitrc.json` 파일을 생성하고 다음과 같이 작성한다.

```json
/* .eslintrc.json - 기본 세팅 */

{
  "root": true,
  "env": {
    "browser": true,
    "es6": true,
    "node": true
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": "latest",
    "sourceType": "module",
    "project": "./tsconfig.json"
  },
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "plugin:prettier/recommended"
  ],
  "plugins": ["prettier", "@typescript-eslint"],
  "rules": {
    "prettier/prettier": ["error", { "endOfLine": "auto" }]
  }
}
```

```json
/* .eslintrc.json - airbnb 세팅 */

{
  "env": {
    "browser": true,
    "es6": true,
    "node": true
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": "latest",
    "sourceType": "module",
    "project": "./tsconfig.json"
  },
  "extends": ["airbnb", "airbnb-typescript", "plugin:prettier/recommended"],
  "plugins": ["prettier", "@typescript-eslint"],
  "rules": {
    "prettier/prettier": ["error", { "endOfLine": "auto" }]
  }
}
```

프로젝트의 루트 위치에 `.prettierrc.json` 파일을 생성하고 다음과 같이 작성한다.

```json
/* .prettierrc.json */

{
  "singleQuote": false,
  "arrowParens": "always",
  "semi": true,
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "trailingComma": "all"
}
```

<br/>
