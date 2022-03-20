---
title: 'Error: Router.use() requires a middleware function but got a Object'
date: 2021-12-24 21:27:00
category: Error
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNail.gif)

# ⚠️ 에러 내용

Express를 실행할 때 다음과 같은 에러가 발생했다.

![그림1. Router.use() requires a middleware function but got a Object 에러](./images/node-router-01.png)

# 📌 에러 원인

`app.js`에서 `express.Router()`를 사용해서 분리해준 파일에서 `export` 해주지 않았다.

# ✅ 해결 방법

`module.exports` 부분을 추가하면 정상 동작한다.

![그림2. Router.use() requires a middleware function but got a Object 해결](./images/node-router-02.png)

<br/>
