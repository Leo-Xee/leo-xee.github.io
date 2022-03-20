---
title: MacOS에 코딩테스트를 위한 C++ 환경 구축하기
date: 2021-12-25 18:23:00
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

![](./images/thumbNail.gif)

이 글에서는 MacOS에 C++로 알고리즘 문제를 풀기 위한 환경을 구축하는 방법을 정리합니다. 물론 VSC의 extension을 설치하는 방법도 있지만 이 글에서는 터미널 환경에서 GCC를 설치하고 설정하는 내용을 다룹니다.

# GCC 설치하기

Homebrew로 gcc를 설치한다.
gcc는 GNU Compiler Collection의 약자이며 C와 C++등의 언어를 지원하는 컴파일러들 중에 하나이다.

```bash
$ brew update
$ brew install gcc
```

설치 완료 후, 원하는 디렉토리를 하나 만들고 해당 디렉토리로 이동한다.
나는 PS라는 디렉토리를 만들고 여기에서 문제들을 푼 소스코드를 관리할 것이다.

```bash
$ mkdir PS
$ cd PS
```

PS 디렉토리에 vi 편집기로 컴파일러의 동작을 확인해볼 `test.cpp` 파일을 생성하고

```bash
$ vi test.cpp
```

아래의 소스코드를 입력한 후에 `:wq`로 저장하고 종료한다.

```cpp
/* test.cpp */

#include <bits/stdc++.h>
using namespace std; int main() {
  cout << "Hello World" << "\n";
    return 0;
}
```

저장한 `test.cpp` 파일을 아래의 명령어로 컴파일한다.

```bash
$ g++ -std=c++14 -Wall test.cpp
```

- `g++` : gcc로 C++을 컴파일( C를 컴파일 하기위해서는 gcc를 사용)
- `-std=c++14` : C++14 버전으로 컴파일
- `-Wall` : 엄격한 규칙으로 컴파일

# bits/stdc++.h 헤더 파일 사용하기

위의 에러를 해결하기 전에 간략히 설명하자면 `bits/stdc++.h`는 모든 표준 라이브러리의 선언을 포함하는 헤더파일이다. 이를 사용하면 알고리즘을 풀 때 자주 사용하는 라이브러리들을 미리 컴파일 해주기 때문에 일일이 추가해줄 필요는 없어진다.

하지만 전체 소프트웨어공학적으로 보면 사용하지 않는 라이브러리도 모두 컴파일하기 때문에 시간이나 공간을 낭비하게 된다는 단점이 있다. 하지만 우리는 각각의 알고리즘 문제를 풀기 위함이기 때문에 큰 문제는 없다.

`bits/stdc++.h`는 아래과 같이 사용한다.

```bash
#include <bits/stdc++.h>
```

`bits/stdc++.h`는 표준 라이브러리가 아니기 때문에 따로 작업을 해줘야한다. 그래서 에러가 발생한 것이다.
이를 위해 아래 명령어로 `/usr/local/include`로 이동한 후에 `bits`라는 디렉토리를 생성한다.

```bash
cd /usr/local/include
mkdir bits
```

vi 편집기로 `bits` 안에 `stdc++.h` 파일을 생성하고

```bash
$ vi stdc++.h
```

해당 파일에 아래의 소스코드를 붙여 넣은 다음 `:wq`로 저장하고 종료한다.

```script
#ifndef _GLIBCXX_NO_ASSERT
#include <cassert>
#endif

#include <cctype>
#include <cerrno>
#include <cfloat>
#include <ciso646>
#include <climits>
#include <clocale>
#include <cmath>
#include <csetjmp>
#include <csignal>
#include <cstdarg>
#include <cstddef>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>

#if __cplusplus >= 201103L
#include <ccomplex>
#include <cfenv>
#include <cinttypes>
#include <cstdbool>
#include <cstdint>
#include <ctgmath>
#include <cwchar>
#include <cwctype>
#endif

// C++
#include <algorithm>
#include <bitset>
#include <complex>
#include <deque>
#include <exception>
#include <fstream>
#include <functional>
#include <iomanip>
#include <ios>
#include <iosfwd>
#include <iostream>
#include <istream>
#include <iterator>
#include <limits>
#include <list>
#include <locale>
#include <map>
#include <memory>
#include <new>
#include <numeric>
#include <ostream>
#include <queue>
#include <set>
#include <sstream>
#include <stack>
#include <stdexcept>
#include <streambuf>
#include <string>
#include <typeinfo>
#include <utility>
#include <valarray>
#include <vector>

#if __cplusplus >= 201103L
#include <array>
#include <atomic>
#include <chrono>
#include <condition_variable>
#include <forward_list>
#include <future>
#include <initializer_list>
#include <mutex>
#include <random>
#include <ratio>
#include <regex>
#include <scoped_allocator>
#include <system_error>
#include <thread>
#include <tuple>
#include <typeindex>
#include <type_traits>
#include <unordered_map>
#include <unordered_set>
#endif
```

# 동작 확인하기

이제 모든 설치 및 세팅을 완료했다. 이전에 오류가 발생 했던 PS 디렉토리 내의 `test.cpp` 파일을 아래의 명령어로 다시 컴파일 해보자.

```bash
$ g++ -std=c++14 -Wall test.cpp -o test.out
```

그러면 `test.out` 이라는 컴파일된 파일을 생성된다.

```bash
.
├── test.cpp
└── test.out
```

이제 아래의 명령어로 컴파일된 `test.out`을 실행하면 정상적으로 `Hello Wolrd`가 출력되는 것을 확인 할 수 있다.

```bash
./test.out
```

<br/>
