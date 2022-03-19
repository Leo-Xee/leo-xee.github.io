---
title: '[Node] Warning: bodyParser was deprecated'
date: 2021-12-09 10:38:00
category: Error
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNail.gif)

# ⚠️ 에러 내용

강의를 보다가 bodyParser 라이브러리를 설치하고 아래와 같이 사용하니까 경고가 발생했다.

```js
const bodyParser = require('bodyParser');
app.use(bodyParser.urlencoded({ extended: true });
app.use(bodyParser.json());
```

```bash
bodyParser was deprecated.
```

# 📌 에러 원인

ExpressJS v4.16부터는 더이상 bodyParser가 지원되지 않고 해당 라이브러리를 내부에 포함하고 있다.

참고로 bodyParser는 request의 body를 파싱해주는 역할을 한다. 추가로 여기서 사용된 메소드도 알아보자면 `urlencoded()`의 `extended`옵션은 URL을 어떤 라이브러리로 파싱할지를 선택하는 옵션이다. `true`이면 `qs`를 `false`이면 `querystring`을 사용한다. 이 둘의 차이점이 궁금하다면 [여기](https://stackoverflow.com/questions/29136374/what-the-difference-between-qs-and-querystring/50199038)에 잘 정리되어 있으니 확인해보자.

간단히 차이를 정리하면 `qs`는 중첩된 객체를 파싱할 수 있지만 `querystring`은 거의 불가능하다.

`json()` 은 이름 그대로 JSON형태의 데이터를 파싱해준다.

# ✅ 해결 방법

다음과 같이 express 내부의 메소드를 사용하면 해결된다.

```js
const express = require('express');
app.use(express.urlencoded({ extended: true });
app.use(express.json());
```

<br/>
