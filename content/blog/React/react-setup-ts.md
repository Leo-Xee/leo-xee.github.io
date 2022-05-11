---
title: 밑바닥부터 React 개발 환경 구축하기 - 2. Typescript
date: 2022-02-05 07:40:00
category: React
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNails/React.gif)

이 글에서는 밑바닥부터 React 개발 환경 구축하기 1편에 이어서 Typescript를 설치 및 적용하는 과정을 정리합니다.

# 시리즈

- [밑바닥부터 React 개발 환경 구축하기 - 1. Webpack](https://leo-xee.github.io/React/react-setup-webpack/)
- 밑바닥부터 React 개발 환경 구축하기 - 2. Typescript
- [밑바닥부터 React 개발 환경 구축하기 - 3. Eslint와 Prettier](https://leo-xee.github.io/React/react-setup-eslint/)

# 타입스크립트 관련 패키지 설치

먼저 타입스크립트를 위해서 필요한 패키지들을 설치한다.

```bash
$ yarn add -D typescript @babel/preset-typescript ts-loader @types/react @types/react-dom fork-ts-checker-webpack-plugin
```

- `typescript` : 타입스크립트 파일(.ts, .tsx)을 자바스크립트 파일로 변환
- `@babel/preset-typescript` : Babel이 타입스크립트를 빌드하기 위해 필요
- `ts-loader` : webpack이 타입스크립트 파일을 로딩할 수 있도록 변환
- `@type/react` : react 관련 API의 타입을 정의
- `@type/react-dom` : react-dom 관련 API의 타입을 정의
- `fork-ts-checker-webpack-plugin` : 타입 체크 과정을 별도의 프로세스에서 실행
  (ts-loader에 transplieOnly: true 옵션을 주고 이 플러그인을 사용하면 .ts 파일을 .js로 변환하는 과정과 타입 체크하는 과정이 병렬적으로 동작해서 빌드 속도를 높일 수 있다.)

# 타입스크립트 설정

프로젝트의 루트 위치에 `tsconfig.json` 을 생성하고 다음과 같이 설정한다.

```json
/* ts.config.json */

{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "sourceMap": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.tsx"],
  "exclude": ["node_modules"]
}
```

# Webpack 설정 추가

Webpack이 타입스크립트를 인식할 수 있도록 `webpack.config.js` 파일의 내용을 다음과 같이 수정한다.

```js
/* webpack.config.js */

const HtmlWebpackPlugin = require('html-webpack-plugin')
const CopyWebpackPlugin = require('copy-webpack-plugin')
const ForkCheckerWebpackPlugin = require('fork-ts-checker-webpack-plugin')
const path = require('path')

module.exports = {
  mode: 'development',
  entry: './src/index.tsx',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: './index.html',
    }),
    new CopyWebpackPlugin({
      patterns: [
        { from: '/src/assets', to: 'assets/', noErrorOnMissing: true },
      ],
    }),
    new ForkCheckerWebpackPlugin(),
  ],
  resolve: {
    modules: [path.resolve(__dirname, 'src'), 'node_modules'],
    extensions: ['.ts', '.tsx', '.js', '.jsx'],
    alias: {
      '@': path.resolve(__dirname, 'src/'),
    },
  },
  devServer: {
    client: {
      overlay: true,
      progress: true,
    },
    compress: true,
    hot: true,
    open: true,
    port: 3000,
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx|ts|tsx)$/,
        exclude: /node_modules/,
        use: [
          'babel-loader',
          {
            loader: 'ts-loader',
            options: {
              transpileOnly: true,
            },
          },
        ],
      },
      { test: /\.css$/, use: ['style-loader', 'css-loader'] },
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: { loader: 'file-loader', options: { name: '[name].[ext]' } },
      },
    ],
  },
}
```

# Babel 설정 추가

Babel이 타입스크립트를 인식할 수 있도록 `babel.config.json` 파일의 내용을 다음과 같이 수정한다.

```json
/* babel.config.json */

{
  "presets": [
    "@babel/preset-react",
    "@babel/preset-env",
    "@babel/preset-typescript"
  ]
}
```

# 프로젝트의 확장자를 tsx로 변경

이제 타입스크립트를 위한 환경 설정을 마쳤으니 프로젝트 내의 모든 `jsx` 확장자명을 `tsx`로 다음과 같이 수정한다.

```tsx
/* index.tsx */

import React from 'react'
import ReactDom from 'react-dom'
import App from './App'

ReactDom.render(<App num={1234} />, document.getElementById('root'))
```

```tsx
/* App.tsx */

import React from 'react'

type AppProps = { num: number }

const App = ({ num }: AppProps) => {
  return <div>Hello, React-App {num}</div>
}

export default App
```

마지막으로 명령어로 다음 명령어로 프로젝트를 실행해보면 잘 동작하는 것을 확인 할 수 있다.

```bash
$ yarn dev
```

# 참조

- https://www.codementor.io/@rajjeet/getting-started-with-typescript-4-easy-steps-1do2bnxl6i
- https://webpack.kr/guides/typescript/
- https://github.com/TypeStrong/fork-ts-checker-webpack-plugin

<br/>
