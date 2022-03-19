---
title: Hello
date: 2022-03-19 07:03:23
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

# 데이터 타입의 분류

자바스크립트의 데이터 타입은 다음과 같이 크게 2가지로 나누어진다.

기본형 타입(Primitive Type)

- Number
- String
- Boolean
- Null
- Undefined
- Symbol

참조형 타입(Reference Type)

- Object
- Array
- Function
- RegExp
- Set/WeakSet
- Map/WeakMap

```tsx
/* /src/components/ProductList.tsx */

import React, { useState, useEffect } from 'react'
import axios from 'axios'

import ErrorBanner from './ErrorBanner'
import { Product } from '../mocks/handlers'

function ProductList() {
  const [items, setItems] = useState<Product[]>([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState(false)

  useEffect(() => {
    loadItems()
  }, [])

  const loadItems = async () => {
    try {
      setLoading(true)
      let res = await axios.get<Product[]>(`http://localhost:3000/products`)
      setLoading(false)
      setItems(res.data)
    } catch (err) {
      setError(true)
    }
  }

  if (loading) return <div>로딩 중입니다.</div>
  if (error) return <ErrorBanner msg="에러가 발생했습니다." />

  return (
    <ul>
      {items.map((item, idx) => (
        <li key={idx}>
          <img src={item.imagePath} alt={`${item.name} product`} />
          {item.name}
        </li>
      ))}
    </ul>
  )
}

export default ProductList
```
