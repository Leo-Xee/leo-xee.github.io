---
title: Emotion
date: 2022-05-04 15:05:63
category: CSS
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNails/CSS.gif)

이 글에서는 Next.js와 Typescript 환경에 Emotion을 적용하고 GlobalStyle, Theming 그리고 폰트 적용과 같은 주요 기능의 사용법을 정리합니다.

# Emotion

**Emotion은 개발자 친화적인 CSS-in-JS 라이브러리이다.** 일반적으로 CSS-in-JS의 생태계에서 Styled-Components와 Emotion이 양분하고 있는데 [npm treads](https://www.npmtrends.com/@emotion/styled-vs-styled-components-vs-@emotion/react)를 통해 확인해보면 Emotion이 좀 더 많이 사용되고 있다.Emotion이 Styled-Components보다 빌드 사이즈가 조금 작지만 유의미한 차이는 아니다.

Next.js에서의 차이라면 Styled-Components를 사용하기 위해서는 추가적인 설정이 필요한 반면에 Emotion은 필요없다는 것이다. 이런 특징 때문에 개발자 친화적이라고 소개하고 있는 것 같은데 최근에는 Next.js에서 Styled-Components를 위한 설정도 제공하고 있어서 사용함에 있어서 두 라이브러리의 차이는 거의 없어지는 것 같다.

# 설치

Next.js에서 Emotion을 사용하기 위해서 다음과 같은 패키지들을 설치한다.

```bash
# react에서 emotion을 사용하기 위한 패키지들
$ yarn add @emotion/react @emotion/styled
```

Emotion을 Next.js의 Typescript 환경에서 사용하기 위해서는 `tsconfig.json`에 `jsxImportSource` 옵션을 다음과 같이 추가해줘야한다.

```json
/* tsconfig.json */

{
  "compilerOptions": {
    //...
    "jsxImportSource": "@emotion/react"
    //..
  }
}
```

# 사용법

이제 Emotion이 제공하는 주요 기능들 몇 가지를 알아보자. 참고로 정말 기본적인 기능은 공식페이지에서 금방 이해할 수 있으니 제외하겠다.

## Global Styles

**Emotion은 모든 페이지에 특정한 스타일을 사용할 수 있는 글로벌 스타일 기능을 제공한다.** 보통 브라우저의 기본 스타일을 제거하기 위해서 `reset.css` 를 적용할 때 사용하는 편이다. 그럼 `reset.css` 를 한번 적용해보자.

먼저 Emotion에서 글로벌 스타일을 적용하기 위해서는 `GlobalStyle.tsx` 파일을 생성하고 다음과 같이 작성한다. 참고로 파일명은 달라도 상관없다.

```tsx
/* GlobalStyle.tsx */

import { css, Global } from '@emotion/react'

const style = css`
  /* reset.css 관련 코드 작성 */
`

function GlobalStyle() {
  return <Global styles={style} />
}

export default GlobalStyle
```

그리고 어떤 `reset.css` 를 사용할 지에 대해서는 [여기](https://velog.io/@teo/2022-CSS-Reset-%EB%8B%A4%EC%8B%9C-%EC%8D%A8%EB%B3%B4%EA%B8%B0)를 확인해보자.

작성한 글로벌 스타일을 적용하기 위해서는 `page` 디렉토리 내부에 `_app.tsx` 파일을 생성하고 다음과 같이 작성하면 잘 적용될 것이다.

```tsx
/* _app.tsx */

import type { AppProps } from "next/app";
import GlobalStyle from "@/styles/GlobalStyle";

function App({ Component, pageProps }: AppProps) {
  return (
    <>
      <GlobalStyle />
      <Component {...pageProps} />
    </>
  );
}

export default App;
```

## Theming

**Emotion은 스타일값 사용을 도와주는 테마 기능 제공한다.** 이는 스타일링된 모든 컴포넌트 내부에서 `props.theme` 로 접근해서 미리 변수에 정의한 스타일 값을 쉽게 사용할 수 있도록 해준다. 그럼 이 기능을 사용해서 일반 모드와 다크 모드의 색상 값을 따로 정의하고 사용해보자.

먼저 테마별 스타일값을 정의하기 위해서 `theme.ts` 파일을 생성하고 다음과 같이 작성한다.

```tsx
/* theme.ts */

const light = {
  primary: 'hotpink',
}

const dark = {
  primary: 'gold',
}

export default { light, dark }
```

하지만 Typescript에서 사용하기 위해서는 정의한 스타일값의 타입 또한 정의해줘야한다. 그러므로 `styles.d.ts` 파일을 생성하고 다음과 같이 타입을 정의한다.

```tsx
/* styles.d.ts */

import '@emotion/react'

declare module '@emotion/react' {
  export interface Theme {
    light: {
      primary: string
    }
    dark: {
      primary: string
    }
  }
}
```

정의한 테마를 사용하기 위해서 `pages` 디렉토리 안에 있는 `_app.tsx` 파일의 컴포넌트를 `ThemeProvider`로 감싸준다.

```tsx
/* _app.tsx */

import type { AppProps } from "next/app";
import { ThemeProvider } from "@emotion/react";
import GlobalStyle from "@/styles/GlobalStyle";
import theme from "@/styles/theme";

function App({ Component, pageProps }: AppProps) {
  return (
    <>
      <GlobalStyle />
      <ThemeProvider theme={theme}>
        <Component {...pageProps} />
      </ThemeProvider>
    </>
  );
}

export default App;
```

이제 정의한 테마가 잘 동작하는 지 확인하기 위해서 `pages` 디렉토리의 `index.tsx` 파일에 다음과 같이 간단한 스타일을 적용하고 실행해보자. 그럼 스타일이 정상적으로 반영되어 있는 것을 확인할 수 있을 것이다.

```tsx
/* index.tsx */

import styled from '@emotion/styled'

const Heading = styled.h1`
  color: ${({ theme }) => theme.light.primary};
`

function HomePage() {
  return (
    <div>
      <Heading>Hello Next.js</Heading>
    </div>
  )
}

export default HomePage
```

## Font

**프로젝트에 웹 폰트를 적용하는 방법은 `<link>`, `@import`로 2가지이다.** 여기서는 [구글 폰트](https://fonts.google.com/)를 이용했다.

`<link>` 로 폰트를 적용하기 위해서는 `pages` 디렉토리 내부에 `_document.tsx` 파일을 생성하고 다음과 같이 `<Head>` 태그 안에 복사한 태그를 위치시키고 `GlobalStyle.tsx` 에 `font-family` 를 적용해주면 된다.

```tsx
/* _document.tsx */

import { Html, Head, Main, NextScript } from 'next/document'

export default function Document() {
  return (
    <Html lang="ko">
      <Head>
        <link rel="preconnect" href="https://fonts.googleapis.com" />
        <link
          rel="preconnect"
          href="https://fonts.gstatic.com"
          crossOrigin="true"
        />
        <link
          href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR&display=swap"
          rel="stylesheet"
        />
      </Head>
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```

```tsx
/* GlobalStyle.tsx */

import { css, Global } from '@emotion/react'

const style = css`
  // ...

  // 폰트 적용
  font-family: 'Noto Sans KR', sans-serif;
`

function GlobalStyle() {
  return <Global styles={style} />
}

export default GlobalStyle
```

`@import` 로 폰트를 적용하기 위해서는 앞서 글로벌 스타일을 적용했던 `GlobalStyle.tsx` 파일에 다음과 같이 폰트의 링크를 추가한 다음에 `font-family` 를 적용해주면 된다.

```tsx
/* GlobalStyle.tsx */

import { css, Global } from '@emotion/react'

const style = css`
  // ...

  // 폰트 추가
  @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR&display=swap');

  // 폰트 적용
  font-family: 'Noto Sans KR', sans-serif;
`

function GlobalStyle() {
  return <Global styles={style} />
}

export default GlobalStyle
```

# 참조

- [https://emotion.sh/docs/introduction](https://emotion.sh/docs/introduction)
