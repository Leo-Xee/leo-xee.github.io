---
title: '[TS] 프로젝트에서 타입 관리하기'
date: 2022-05-16 18:05:85
category: Typescript
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNail.gif)

React 및 Next.js 프로젝트를 Typescript로 구현하면서 각기 다른 위치에 커스텀 타입 선언이 생겨나서 관리가 힘들어지는 문제가 발생했습니다. 이러한 문제를 해결하기 위한 방법을 정리합니다.

# 문제

React와 Next.js 같은 프로젝트에서 Typescript를 일반적으로 사용하기 시작하면서 컴포넌트 파일 및 다른 파일들의 내부에 커스텀 타입 정의들이 중구난방으로 생겨나기 시작했다. 물론 export만 잘해준다면 IDE가 잘 찾아주겠지만 나는 타입 파일들은 타입대로 하나의 폴더에서 관리해서 사용하고 싶었다.

# 타입 관련 파일 분리하기

일단 타입 관련된 파일을 분리하기 위해서 프로젝트 루트 위치의 `src` 디렉토리 내부에 `@types`라는 디렉토리를 생성한다. 이제부터 컴포넌트의 Props의 타입을 제외한 모든 타입을 `@types` 디렉토리에서 관리할 것이다.

이를 위해서는 Typescript 컴파일러가 인식할 수 있도록 다음과 같이 `@types`를 포함해주어야 한다.

```tsx
/* tsconfig.json */

{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
		// ...
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", "./src/@types"],  // 추가
  "exclude": ["node_modules"]
}
```

# .ts 와 .d.ts

Typescript에서는 타입을 정의할 때, 파일의 확장자로 `.ts` 와 `.d.ts` 를 사용할 수 있다. **`.ts` 는 Javascript로 컴파일되는 일반적인 타입스크립트 파일이고 `.d.ts`는 선언한 내용이 Typescript 컴파일 시의 문맥에 자동으로 추가되지만 자바스크립트로 컴파일되지는 않는 파일이다. 참고로 `.d.ts` 로 정의된 선언을 Ambient라고 부르고 이는 구현을 정의하지 않은 선언을 의미한다.** 이에 대한 자세한 내용은 [여기](https://stackoverflow.com/questions/29196657/what-is-the-difference-between-d-ts-vs-ts-in-typescript)를 참고하자.

`.ts` 나 `.d.ts` 를 사용하던지 큰 차이는 없지만 `.d.ts` 를 사용하면 `module` 키워드를 사용해서 절대 경로로 `import` 할 수 있는 장점이 있다. 그래서 나는 `.d.ts` 파일을 사용해서 타입 정의를 할 것이다. 이에 대한 예시는 다음과 같다.

```tsx
/* src/@types/<something>.d.ts */

declare module "<something>" {
	export type Leo: string;
	interface Ryan : {
		name: string
	}
}

```

```tsx
import { Leo } from '<something>'

const friend: Leo = "leo's friend"
```

이제 프로젝트에서 Typescript의 타입 정의를 하나의 디렉토레에서 관리하면서 보다 쾌적한 환경에서 프로젝트를 구현해보자~!!
😊

# 참고

- [https://typescript-kr.github.io/pages/modules.html](https://typescript-kr.github.io/pages/modules.html)

<br>
