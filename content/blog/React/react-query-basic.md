---
title: React-Query
date: 2022-5-27 22:06:43
category: React
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNails/React.gif)

# React Query란?

![그림. react-query logo](./images/react-query-basic-01.png)

**React-Query는 데이터 패칭, 캐싱, 업데이트, 동기화와 같은 서버 상태를 관리해주는 React 라이브러리이다.** 기존에는 Redux와 같은 전역 상태 관리 라이브러리에서 클라이언트 상태뿐만 아니라 서버 상태까지 관리하고 있어서 로직이 복잡해지는 문제가 있었다. 이를 해결하기 위해 등장한 것이 서버 상태 관리 라이브러리이며 대표적으로 React-Query가 사용된다. 이 외에도 SWR, RTK Query가 있다.

# 설치 및 세팅

```bash
$ yarn add react-query
```

Next.js에서 React-Query를 사용하기 위해서는 `pages/_app.tsx` 에서 `Context Provider`로 `QueryClient` 를 다음과 같이 전달해줘야한다.

```tsx
import { AppProps } from 'next/app'
import { QueryClientProvider, QueryClient } from 'react-query'
import { ReactQueryDevtools } from 'react-query/devtools'

function MyApp({ Component, pageProps }: AppProps) {
  const queryClient = new QueryClient()

  return (
    <QueryClientProvider client={queryClient}>
      <Component {...pageProps} />
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  )
}

export default MyApp
```

# 사용

## useQuery

useQuery는 데이터를 GET하기 위해 사용한다.

```tsx
// 기본 구조
const { isLoading, isError, data, ... } = useQuery(queryKey, queryFn, optionObj);

// queryKey로 문자열을 받는 경우의 예시
const { data } = useQuery("/carts", getCarts);

// queryKey로 배열을 받는 경우의 예시
const { data } = useQuery(["/orders", orderId], () => getOrder(orderId));

// select 옵션을 사용해서 데이터를 filter하는 예시
const { data } = useQuery("/carts", getCarts, {
  select: (carts) => carts.filter((cart) => cart.product.selected),
});
```

- **`queryKey`**

  - 데이터를 GET하는 요청들을 구분하는 Key값을 받으며 이는 해시화되어 관리된다.
  - useQuery는 Key값을 기반으로 캐싱을 하기 떄문에 Key값이 동일하면 다른 데이터에 대한 요청일지라도 같은 요청으로 인식하고 캐시를 관리한다.
  - 타입으로는 문자열이나 배열을 받는다.

- **`queryFn`**

  - GET 요청에 사용할 비동기함수를 받는다.
  - 인자가 필요한 비동기함수일 경우에는 함수 호출문을 넣지않도록 주의해야한다.

- **`OptionObj`**
  - useQuery에서 사용할 옵션 객체를 받으며 주로 사용되는 옵션은 다음과 같다.
    - `onSuccess`, `onError`, `onSettled`: 성공, 실패, 완료 시에 실행할 Side Effect를 정의
    - `enabled`: 자동으로 query를 실행할지 말지 여부를 지정
    - `retry`: query 요청 실패 시에 자동으로 재시도할지 결정
    - `select`: 요청을 성공한 데이터를 가공해서 전달
    - `keepPreviousData`: 새로운 데이터 fetching할 때 이전의 데이터를 유지
    - `refetchInterval`: 주기적으로 refetch할지를 결정
    - `refetchOnWindowFocus`: 브라우저가 다시 포커스 되었을 때 실행할지 말지 여부 지정

## useMutation

useMutation은 데이터를 POST, PUT, PATCH, DELETE하기 위해 사용한다.

```tsx
// 기본 구조
const mutation = useMutation(mutationFn, optionObj)

// 선언 예시
const addCartMutation = useMutation((product: Product) => addCart(product))

// 실행 예시
addCartMutation.mutate(products)
```

- `mutationFn`

  - POST, PUT, PATCH, DELETE 요청을 수행할 비동기 함수를 받는다.
  - useMutation을 실행하기 위해서는 반환 객체의 메소드인 `mutate`를 사용해야한다.

- `optionObj`
  - useMutation에서 사용할 옵션 객체를 받는다.

## InvalidateQueries

**useMutation으로 서버의 데이터가 변경되었다면 기존의 useQuery로 GET한 데이터는 일반적으로 stale한 상태가 된다.** 이 때 캐시에서 기존의 데이터를 폐기하고 데이터를 다시 불러올 때 `InvalidateQueryies` 메소드가 사용된다.

> 여기서 stale은 상한, 썩은 의미를 가진 단어로 React-Query에서는 더이상 유효하지 않은 데이터를 칭할 때 주로 사용한다.

<br>

이는 위에서 작성한 useMutation 예시의 경우에 데이터 변경에 대한 요청은 성공적으로 완료되지만 동일한 데이터를 GET하는 useQuery는 여전히 이전의 데이터를 보여주고 있음을 의미한다.

그렇다면 Mutation 요청이 성공했을 때 useQuery가 데이터를 다시 불러오도록 수정하면 다음과 같다.

```tsx
import { useMutation, useQueryClient } from 'react-query'
에
const queryClient = useQueryClient()

const addCartMutation = useMutation((product: Product) => addCart(product), {
  // 성공 시에 queryKey가 "/carts"인 useQuery의 캐시 초기화 및 재실행
  onSuccess: () => {
    queryClient.invalidateQueries('/carts')
  },
})
```

- `InvalidateQueries` 메소드는 `useQueryClient`의 반환 객체에 포함되어 있다.

<br>

지금까지는 CSR(Client Side Rendering)을 전제로 React-Query를 사용하는 기본적인 방법을 살펴봤다. 그렇다면 SSR(Server Side Rendering)의 경우에는 어떻게 사용할까?

# Next.js의 SSR에 적용

**React-Query는 친절하게도 Next.js 기반의 SSR에서도 사용할 수 있도록 크게 2가지 방법을 제공한다.** 첫 번째는 `InitialData`를 사용하는 방법이고 두 번째는 `Hydration`을 사용하는 방법이다. 여기서는 일반적으로 사용되는 두 번째 방법에 대해서 정리한다

## 세팅 추가

이를 위해서는 `pages/_app.tsx`에 추가적인 세팅을 다음과 같이 해줘야한다.

```tsx
import { AppProps } from 'next/app'
import { QueryClientProvider, QueryClient, Hydrate } from 'react-query'
import { ReactQueryDevtools } from 'react-query/devtools'

function MyApp({ Component, pageProps }: AppProps) {
  const [queryClient] = useState(() => new QueryClient())

  return (
    <QueryClientProvider client={queryClient}>
      <Hydrate state={pageProps.dehydratedState}>
        <Component {...pageProps} />
      </Hydrate>
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  )
}

export default MyApp
```

## 사용

그 다음, 페이지 컴포넌트의 `GetServerSideProps`의 내부에 다음과 같이 작성해주면 된다.

```tsx
import { GetServerSideProps } from 'next'
import { dehydrate, QueryClient } from 'react-query'

function CartPage() {
  // ...
}

export const getServerSideProps: GetServerSideProps = async () => {
  const queryClient = new QueryClient()

  await queryClient.prefetchQuery('/carts', getCarts)

  return {
    props: {
      dehydratedState: dehydrate(queryClient),
    },
  }
}
```

이제 해당 페이지 컴포넌트 내부에서 사용되는 `useQuery('/carts', getCarts)`는 SSR에서 정상적으로 실행된다.

# 참조

- https://react-query.tanstack.com
- https://www.youtube.com/watch?v=MArE6Hy371c
- https://github.com/Leo-Xee/next-shopping-cart
