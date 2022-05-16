---
title: 'Error: Unknown command: cask'
date: 2021-12-23 19:37:00
category: Error
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNail/thumbNail.gif)

# ⚠️ 에러 내용

Homebrew로 mysql을 설치하다가 다음과 같이 cask 명령어를 인식하지 못하는 에러가 발생했다.

![그림1. Unknown command: cask 에러](./images/unknown-cask-01.png)

# 📌 에러 원인

스택오버플로우의 [이 글](https://stackoverflow.com/questions/30413621/homebrew-cask-option-not-recognized)에서 2021년부터 cask의 사용 방법이 변경되었다는 것을 알 수 있었다.

```bash
# 2021 이전
$ brew cask install <app>

# 2021 이후
$ brew install --cask <app>
```

# ✅ 해결 방법

brew install --cask 를 사용하면 정상적으로 설치된 것을 확인할 수 있었다.

![그림2. Unknown command: cask 해결](./images/unknown-cask-01.png)

<br/>
