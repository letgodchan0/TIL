# 🔰Vue Component

- 기본 HTML 엘리먼트를 확장하여 재사용 가능한 코드를 캡슐화 하는데 도움을 줌
- CS에서는 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어 구성 요소를 의미
- 즉, 컴포넌트는 유지 보수를 쉽게 만들어 줄 뿐만 아니라, 재사용성의 측면에서도 매우 강력한 기능을 제공

<BR>

## 🪴 SFC (Single File Component)

- Vue의 컴포넌트 기반 개발의 핵심 특징
- 하나의 컴포넌트 `.vue` 확장자를 가진 하나의 파일 안에서 작성되는 코드의 결과물
- 화면의 특정 영역에 대한 HTML, CSS, JavaScript 코드를 하나의 파일(.vue)에서 관리
- 즉, `.vue` 확장자를 가진 싱글 파일 컴포넌트를 통해 개발하는 방식
- `Vue 컴포넌트` === `Vue 인스턴스` === `.vue 파일`

<BR>

## 🪴 Vue Component 구조 예시

![image-20220510234640279](Vue%20Component%20&%20CLI.assets/image-20220510234640279.png)

- 한 화면 안에서도 기능 별로 각기 다른 컴포넌트가 존재
  - 하나의 컴포넌트는 여러 개의 하위 컴포넌트를 가질 수 있음
  - Vue는 컴포넌트 기반의 개발 환경 제공
- Vue 컴포넌트는 `const app = new Vue({...})`의 app을 의미하며 이는 Vue 인스턴스
  - 여기서 오해하면 안된느 것은 컴포넌트 기반의 개발이 반드시 파일 단위로 구분되어야 하는 것은 아님
  - 단일 `.html` 파일 안에서도 여러 개의 컴포넌트를 만들어 개발 가능

<BR>

## 🪴 Vue CLI

- Vue.js 개발을 위한 표준 도구
- 프로젝트의 구성을 도와주는 역할을 하며 Vue 개발 생태계에서 표준 tool 기준을 목표로 함
- 확장 플러그인, GUI, Babel 등 다양한 tool 제공

<br>



## 🪴 Node.js

- 자바스크립트를 브라우저가 아닌 환경에서도 구동할 수 있도록 하는 자바스크립트 런타임 환경
  - 브라우저 밖을 벗어 날 수 없던 자바스크립트 언어의 태생적 한계를 해결
- Chorme V8 엔진을 제공하여 여러 OS 환경에서 실행할 수 있는 환경을 제공
- 즉, 단순히 브라우저만 조작할 수 있던 자바스크립트를 SSR 아키텍처에서도 사용할 수 있도록 함

<br>



## 🪴 NPM (Node Package Manage)

- 자바스크립트 언어를 위한 패키지 관리자
  - Python에 pip가 있다면 Node.js에는 NPM
  - pip와 마찬가지로 다양한 의존성 패키지를 관리
- Node.js의 기본 패키지 관리자
- Node.js 설치 시 함께 설치됨

<br>



## 🪴 Vue CLI 시작하기

1. 설치

```BASH
$ npm install -g @vue/cli
```

2. 버젼 확인

```bash
$ vue --version
```

3. 프로젝트 생성

```bash
$ vue create my-first-app
```

4. 버전 선택 (Vue 2)

![image-20220510235543343](Vue%20Component%20&%20CLI.assets/image-20220510235543343.png)

5. 프로젝트 성공

![image-20220510235529815](Vue%20Component%20&%20CLI.assets/image-20220510235529815.png)

6. 프로젝트 디렉토리 이동

```bash
$ cd my-first-app
```

7. 서버 실행

```bash
$ npm run serve
```

<br>

## 🪴Babel

- 자바스크립트의 ECMAScript 2015+ 코드를 이전 버전으로 번역/변환해 주는 도구
- 과거 자바스크립트의 파편화와 표준화의 영향으로 코드의 스펙트럼이 매우 다양
  - 이 때문에 최신 문법을 사용해도 이전 브라우저 혹은 환경에서 동작하지 않는 상황이 발생
- 원시 코드(최신 버전)를 목적 코드(구 버전)로 옮기는 번역기가 등장하면서 개발자는 더 이상 내 코드가 특정 브라우저에서 동작하지 않는 상황에 대해 크게 고민하지 않을 수 있게 됨

<br>

## 🪴 Webpack

- "static module bundler"
- 모듈 간의 의존성 문제를 해결하기 위한 도구
- 프로젝트에 필요한 모든 모듈을 매핑하고 내부적으로 종속성 그래프를 빌드함

<hr>

### 🌳 Module 의존성 문제

- 모듈의 수가 많아지고 라이브러리 혹은 모듈 간의 의존성(연결성)이 깊어지면서 특정한 곳에서 발생한 문제가 어떤 모듈 간의 문제인지 파악하기 어려움
- 즉, Webpack은 이 모듈 간의 의존성 문제를 해결하기 위해 등장

<hr>

### 🌳 Static Module Bundler

- 모듈 의존성 문제를 해결해주는 작업을 Bundling이라 함
- 이러한 일을 해주는 도구가 Bundler이고, Webpack은 다양한 Bundler 중 하나
- 여러 모듈을 하나로 묶어주고 묶인 파일은 하나(혹은 여러 개)로 합쳐짐
- Bundling된 결과물은 더 이상 순서에 영향을 받지 않고 동작하게 됨
- snowpack, parcel, rollup.js 등의 webpack 이외에도 다양한 모듈 번들러 존재

<br>



## 🪴 정리

- Node.js
  - JS를 브라우저 밖에서 실행할 수 있는 새로운 환경
- Babel
  - Compiler
  - ES2015+ JavaScript 코드를 구 버전의 JavaScript로 바꿔주는 도구
- Webpack
  - Module Bundler
  - 모듈 간의 의존성 문제를 해결하기 위한 도구

<br>

## 🪴Vue 프로젝트 구조

- node_modules
  - node.js 환경의 여러 의존성 모듈
  - 파이썬의 `venv`라서 git에 올리면 안된다 ㅎㅎ
  - vue 프로젝트를 실행하면 gitignore가 알아서 만들어져 있음..!
- public/index.html
  - Vue 앱의 뼈대가 되는 파일
  - 실제 제공되는 단일 html 파일

- src/assets
  - webpack에 의해 빌드 된 정적 파일
- src/components
  - 하위 컴포넌트들이 위치
- src/App.vue
  - 최상위 컴포넌트
- src/main.js
  - webpack이 빌드를 시작할 때 가장 먼저 불러오는 entry point
  - 실제 단일 파일에서 DOM과 data를 연결 했던 것과 동일한 작업이 이루어지는 곳
  - Vue 전역에서 활용 할 모듈을 등록할 수 있는 파일
- babel.config.js
  - babel 관련 설정이 작성된 파일
- package.json
  - 프로젝트의 종속성 목록과 지원되는 브라우저에 대한 구성 옵션이 포함
- package-lock.json
  - node_modules에 설치되는 모듈과 관련된 모든 의존성을 설정 및 관리
  - 팀원 및 배포 환경에서 정확히 동일한 종속성을 설치하도록 보장하는 표현
  - 사용 할 패키지의 버전을 고정
  - 개발 과정 간의 의존성 패키지 충돌 방지

