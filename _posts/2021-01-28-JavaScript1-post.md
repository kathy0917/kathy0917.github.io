---
layout: post
title: "[JavaScript] async vs defer, use strict"
autor: "kathy"
tags: JavaScript
---

*이 글은 드림코딩 엘리님의 자바스크립트 강의를 참고하여 공부 목적으로 작성한 글입니다.<br>
(출처: https://www.youtube.com/watch?v=tJieVCgGzhs&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=2)

<br>

---

`<script>`를 선언하는 경우에는 총 4가지가 있다.

- `<head>`안에 `<script>`선언
- `<body>`안 끝 부분에 `<script>`선언
- `<head>`안에 async 속성값을 준 `<script>`를 사용하는 경우 (head+async)
- `<head>`안에 defer 속성값을 준 `<script>`를 사용하는 경우 (head+defer)

아래에 각 코드와 장단점에 대해 정리해놓았다.

---

<br>

### 1. `<head>`안에 `<script>`를 선언

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <script src="main.js"></script>
</head>
<body>
</body>
</html>
```

![1](https://user-images.githubusercontent.com/28476745/106025162-d457f200-610b-11eb-8362-e9dc2b6d21ba.png)

<br>

브라우저가 HTML을 한 줄씩 읽으며 DOM요소로 변환 → `<script>`발견 시, main.js를 다운받은 후, 실행 → 다시 parsing한 부분으로 넘어감.


⚠️단점: <br>JavaScript 파일의 크기가 크고 인터넷이 느린 경우, 사용자가 웹사이트를 보는 시간이 오래 걸린다.

<br>
<br>

### 2. `<body>`안 끝 부분에 `<script>`선언

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <h1></h1>
    <script src="main.js"></script>
  </body>
</html>
```

![2](https://user-images.githubusercontent.com/28476745/106024590-45e37080-610b-11eb-9fe9-aae84da7679e.png)

HTML을 쭉 parsing → 페이지가 준비됨. (위 이미지의 분홍색 점선) → `<script>`를 만나서 서버에서 다운받은 후 실행함.

<br>

✅장점:<br>
사용자가 기본적인 컨텐츠를 빨리 볼 수 있다.

⚠️단점:<br>
JavaScript에 의존적인 웹사이트일 경우, 비효율적이다.<br>
(예를 들어, querySelector()를 활용해 DOM요소를 꾸밀 때,
사용자가 정상적인 사이트를 보려면 JavaScript파일을 다운로드받고 실행하는 시간을 기다려야한다. )

<br>
<br>

### 3. `<head>`안에 async 속성값을 준 `<script>`를 사용하는 경우 (head+async)

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
		<script async src="main.js"></script>
</head>
<body>
	<h1></h1>
</body>
</html>
```

![3](https://user-images.githubusercontent.com/28476745/106024678-5b589a80-610b-11eb-80ec-ccdaea457c5e.png)

async는 boolean타입이기 때문에 선언만 해도 true가 된다.

<br>

브라우저가 `<script>`를 만나면 병렬로 JavaScript 파일을 다운받기로 함 → 다운로드가 완료되면 html parsing을 멈춤. → JavaScript 파일을 실행 → 나머지 html파일을 파싱<br>
(위 이미지의 분홍색 점선이 페이지가 준비되는 시점이다.)

<br>

✅장점: <br>
JavaScript파일을 다운로드 받는 시간을 절약할 수 있다.

⚠️단점:<br>

- JavaScript 파일 내 DOM요소를 조작하려는 시점에 HTML 요소가 정의되어 있지 않을 수 있다. <br>
- JavaScript 파일을 다운받는 시점에 parsing이 중단되므로 사용자가 웹페이지 보는 시간이 지연될 수 있다.

<br><br>

### 4. ⭐`<head>`안에 defer 속성값을 준 `<script>`를 사용하는 경우 (head+defer)

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
		<script defer src="main.js"></script>
</head>
<body>
	<h1></h1>
</body>
</html>
```

<center><img src="https://user-images.githubusercontent.com/28476745/106024704-66abc600-610b-11eb-83de-a5fd8a4a7a71.png" width="70%"></center>
<br>

브라우저가 defer를 만나면 JavaScript 파일을 다운로드만 받자고 명령함. → html 파일을 끝까지 parsing 한 후, 마지막에 다운로드된 JavaScript 파일을 실행

<br>

✅html파일을 모두 파싱하고 다운로드된 JavaScript 파일을 바로 실행하므로 가장 좋은 방법이다.

<br>
<br>

### async와 defer의 차이점

- async

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
		<script async src="1.js"></script>
		<script async src="2.js"></script>
		<script async src="3.js"></script>
</head>
<body>
	<h1></h1>
</body>
</html>
```

위처럼 여러 개의 script파일을 정의한 경우,
1.js → 2.js → 3.js 파일 순서대로 실행되지 않을 수도 있다.
다운로드 받아지는 파일을 먼저 실행하므로 2.js가 1.js보다 빨리 실행될 수 있다.

⚠️따라서, 개발자가 정의한 순서대로 JavaScript파일을 실행해야할 경우 문제가 발생한다.

<br>

- defer

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <script defer src="1.js"></script>
    <script defer src="2.js"></script>
    <script defer src="3.js"></script>
  </head>
  <body>
    <h1></h1>
  </body>
</html>
```

JavaScript 파일을 모두 다운로드 받은 후, 순서대로 실행하기 때문에 개발자가 정의한대로 JavaScript가 실행된다.

✅ head + defer 조합이 가장 안전하고 효율적이다.

<br>
<br>

# use strict

JavaScript는 유연한 언어인 동시에 개발자가 실수할 경우 위험한 언어가 될 수 있다.
따라서, 'use strict'는 타입스크립트에서 필요없지만 VanillaJS에서는 사용하는 것이 좋다.

<br>

✅ 장점:
성능 개선 기대 - JavaScript 엔진이 JavaScript를 더 빠르게 분석할 수 있다.

<br>

예시)

```jsx
"use strict";

a = 6;
```

위 예시에서 만약 'use strict'를 선언하지 않으면 error가 발생하지 않는다.
하지만, 'use strict'를 선언한 경우에 개발자가 변수a를 선언하지 않고 값을 할당하면 ReferenceError: a is not defined error가 발생한다.

아래 예시처럼 a라는 변수를 선언해줘야 한다.

```jsx
"use strict";

let a;
a = 6;
```
