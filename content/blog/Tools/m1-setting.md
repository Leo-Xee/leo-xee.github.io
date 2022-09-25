---
title: M1 Macbook 초기 세팅하기
date: 2022-09-25 18:09:14
category: tools
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNails/Tools.gif)

이 글에서는 최근에 회사에서 지급 받은 M1 Macbook Pro를 초기 세팅하면서 겪었던 병목을 다시 겪지 않고자 세팅 내용을 간단히 정리합니다.

# MacOS 설정

- `General`

  - `Appearance` : Dark로 변경
  - `Accent color` : Pink로 변경

- `Desktop & Screen Saver`

  - `Show screen saver after` : 1시간으로 변경

- `Dock & Menu Bar`

  - `Size` : 25%로 변경
  - `Magnification` : Max로 변경
  - `Show recent applications in Dock` : 해제

- `Keyboard`

  - `Keyboard` - `Key Repeat` : 100% 변경
  - `Shorcuts` - `Spotlight` : 모두 해제 (Alfred 사용)
  - `Input Sources` - `Caps Lock key to switch to and from ABC` : 해제

- `Trackpad`

  - `Tracking speed` : 100% 변경

- `Battery`
  - `Power Adapter` : 디스플레이 끄기 1시간으로 변경

</br>

# Homebrew

[Homebrew](https://brew.sh/index_ko)는 패키지들을 쉽게 관리할 수 있도록 도와주는 Mac용 패키지 매니저이다.

## 설치

```bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

</br>

# Iterm2

[Iterm2](https://iterm2.com/) 는 MacOS에서 기본으로 제공하는 터미널보다 훨씬 많은 기능을 제공하는 터미널 애플리케이션이다. 여기에서는 Iterm2에 Dracula 테마를 적용할 예정이다.

## 설치

```bash
$ brew install iterm2
```

## 로제타 전용으로 복제 (필요 시)

Iterm2가 M1에 완전하게 호환되지 않는 경우를 대비해서 Iterm2를 복제해서 로제타로 실행해서 사용한다.

1. `Finder` - `Applications` 에서 Iterm을 우클릭 후에 `Duplicate`한다.
2. 복제한 Iterm의 이름을 Iterm Rosetta로 변경한다.
3. Iterm Rosetta를 우클릭 후에 `Get info` 한다.
4. `Open using Rosetta` 를 체크한다.
5. Iterm Rosetta를 메인으로 사용한다.

## 테마 적용

1. Dracula 테마 프리셋을 다음 명령어로 다운로드한다.

```bash
$ git clone https://github.com/dracula/iterm.git
```

2. 다운 받은 `Dracula.itermcolors` 을 실행해서 `import` 한다.
3. `Preference` - `Profile`
   - `Colors` 에서 `Color Presets` 를 Dracula로 변경
   - `Window` 에서 `Blur` 체크 후 25로 변경하고 `Transparency` 를 30으로 변경

## 폰트 적용

1. Fira Code 폰트를 [GitHub 페이지](https://github.com/tonsky/FiraCode)에서 다운로드한다.
2. 다운 받은 Fira Code를 설치한다.
3. `Preference` - `Profile`
   - `Text`에서 `Font`를 Fira Code로 변경

# Oh My Zsh

[Oh My Zsh](https://ohmyz.sh/)는 Zsh를 쉽게 관리해주는 커뮤니티 기반 프레임워크이다. 이를 사용하면 다양한 플러그인과 테마를 사용할 수 있다.

## 설치

`Oh My Zsh`를 다음 명령어로 설치한다.

```bash
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 플러그인 설치

### autojump

autojump 플러그인을 다음 명령어로 설치한다.

```bash
$ brew install autojump
```

설치 후에 `~/.zshrc`의 plugins에 autojump를 추가한다.

```shell
plugins=(git autojump)
```

### zsh-syntax-highlighting

syntax-highlighting 플러그인을 다음 명령어로 설치한다.

```bash
$ brew install zsh-syntax-highlighting
```

설치 후에 `~/.zshrc`의 최하단에 다음 코드를 추가한다.

```shell
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

### zsh-autosuggestions

zsh-autosuggestions 플러그인을 다음 명령어로 설치한다.

```bash
$ brew install zsh-autosuggestions
```

설치 후에 `~/.zshrc`의 최하단에 다음 코드를 추가한다.

```shell
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
```

</br>

# Karabiner

[Karabiner](https://karabiner-elements.pqrs.org/)는 손쉽게 키 맵핑을 커스터마이징할 수 있는 애플리케이션이다. [커뮤니티](https://ke-complex-modifications.pqrs.org/)에서 제공하는 프리셋을 사용해서 쉽게 원하는 조합으로 쉽게 세팅할 수 있다.

## 설치

```bash
$ brew install karabiner-elements
```

## 세팅

저는 Vim을 사용하기 때문에 다음과 같은 프리셋과 설정을 사용한다.

- `CAPS_LOCK to Hyper/Escape & Hyper + VIM Navigation keys`
- `Vimode with smart caps`
- `For Korean`
- `For Korean PC Keyboard`
- `F4` 를 `Launchpad` 로 변경

</br>

# 주요 패키지들

## Node

```bash
# 설치
$ brew install node

# 버전 확인
$ node -v
```

## Yarn

```bash
# 설치 (이미 설치한 node 제외 옵션)
$ brew install yarn --ignore-dependencies

# 버전 확인
$ yarn -v
```

## NVM

NVM을 다음 명령어로 설치한다.

```bash
$ brew install nvm
```

NVM을 위한 디렉토리를 생성한다.

```bash
$ mkdir ~/.nvm
```

`~/.zshrc` 의 최하단에 다음 코드를 추가한다.

```shell
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
```

정상적으로 설치 되었는지 확인한다.

```bash
$ nvm -version
```

<br/>

# 주요 애플리케이션들

```bash
$ brew install <필요한 애플리케이션>
```

다음은 주로 사용하는 애플리케이션 목록이다.

- visual-studio-code
- alfred
- chrome
- firefox
- notion
- figma
- sourcetree
- slack
- obsidian
- app cleaner
- spectacle
- runcat
