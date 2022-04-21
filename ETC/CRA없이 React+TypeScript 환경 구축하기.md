# 📌 시작하게된 계기

프로젝트를 진행함에 있어
CRA 없이 React App을 만들어보자는
좋은 아이디어를 듣고

CRA를 사용하지 않고
React + TypeScript 환경을 구축해보기로 했다.

## 🛠 필요한 설정

### React 설정

```
npm init -y
npm i react react-dom
```

> 프로젝트를 구축함에 있어 react, react-dom 패키지를 먼저 설치한다.

#### index.html

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React App without CRA</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

#### index.tsx

```javascript
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

const rootElement = document.getElementById("root");

ReactDOM.render(<App />, rootElement);
```

#### App.tsx

```javascript
import React from "react";

const App = () => (
  <>
    <h1>React without CRA...</h1>
  </>
);

export default App;
```

> React에 기본적으로 필요한
> index.html, index.tsx, App.tsx 파일들을 생성한다.

</br>

### TypeScript 설정

```
npm i -D typescript @types/react @types/react-dom
tsc --init
```

> TypeScript를 사용하기에,
> TypeScript 관련 패키지를 설치한 후
> 타입스크립트 설정 파일을 초기화한다.

</br>

#### tsconfig.json

```javascript
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "lib": ["dom", "ES2015", "ES2016", "ES2017", "ES2018", "ES2019", "ES2020"],
    "allowJs": true,
    "jsx": "preserve",
    "sourceMap": true,
    "outDir": "./dist",
    "isolatedModules": true,
    "strict": true,
    "moduleResolution": "node",
    "baseUrl": "./",
    "paths": {
      "@components/*": ["components/*"],
      "@pages/*": ["pages/*"]
    },
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true
  }
}

```

> 자신이 원하는 스타일대로 설정을 한다.
> tsconfig.json의 기능들은 따로 정리를 했기에
> 특정 설정 및 기능이 궁금하다면 해당 글을 확인하면 된다.

[tsconfig.json 정리](https://velog.io/@tnehd1998/tsconfig.json-%EB%82%B4%EC%9A%A9-%EC%A0%95%EB%A6%AC)

</br>

### Babel 설정

다음과 같은 패키지를 설치한다.

```javascript
npm i -D babel-loader @babel/core @babel/preset-env
npm i -D @babel/preset-react @babel/preset-typescript

```

각 패키지는 다음과 같은 역할을 수행한다.

#### babel-loader, @babel/core, @babel/preset-env

> - 보통 ES6+문법으로 코드를 작성하면 IE 웹 브라우저와 같이 구형 브라우저에서는
>   제대로 지원이 되지 않는다.
> - 그렇기에 babel JavaScript 컴파일러를 활용하여
>   IE에서도 ES6+ 문법을 사용할 수 있게 한다.

</br>

#### @babel/preset-typescript

> - 원래 바벨과 타입스크립트는 따로 작업이 되었지만,
>   해당 플러그인을 통해 타입스크립트와 바벨이 조화롭게 병합하여 사용하게 된다.
> - TypeScript를 사용한다면 필요한 플러그인들의 집합이다.

</br>

#### @babel/preset-react

> - jsx로 작성된 코드들을 createElement 함수를 이용한 코드로 변환해 주는
>   바벨 플러그인이 내장되어 있음
> - React를 사용한다면 필요한 플러그인들의 집합이다.

</br>

#### babel.config.js

```javascript
module.exports = {
  presets: [
    "@babel/preset-react",
    "@babel/preset-env",
    "@babel/preset-typescript",
  ],
};
```

> 패키지 설치를 한 후,
> 해당 코드를
> 바벨 설정 파일을 의미하는
> babel.config.js에 입력한다.

</br>

### Webpack 설정

다음과 같은 패키지를 설치한다.

```
npm i -D webpack webpack-cli webpack-dev-server webpack-merge
npm i -D html-webpack-plugin ts-loader
```

각 패키지는 다음과 같은 역할을 수행한다.

#### webpack, webpack-cli

> - webpack을 사용하기 위해서 기본적으로 필요한 패키지

    - webpack : 웹팩 그 자체의 패키지
    - webpack-cli : 터미널에서 webpack 커맨드를 실행할 수 있게 해주는 커맨드라인 도구

</br>

#### webpack-dev-server

> - 빠른 실시간 리로드를 가능하게 하는 개발 서버

    - 디스크에 저장되지 않는 메모리 컴파일을 사용하기 때문에 컴파일 속도가 빠름
    - webpack.config.js에서 devServer 옵션을 통해 옵션을 지정하여 사용이 가능함

</br>

#### webpack-merge

> - webpack을 dev, prod 모드로
>   편안하게 분리하여 구축할 수 있게 도와준다.

</br>

#### html-webpack-plugin

> - html 파일을 템플릿으로 생성할 수 있게 도와주는 플러그인

</br>

#### ts-loader

> - 타입스크립트 코드를 자바스크립트 코드로 변환

#### webpack.common.js

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const webpack = require("webpack");
const path = require("path");

module.exports = {
  entry: "./src/index.tsx",
  resolve: {
    extensions: [".js", ".jsx", ".ts", ".tsx"],
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: ["babel-loader", "ts-loader"],
      },
    ],
  },
  output: {
    path: path.join(__dirname, "/dist"),
    filename: "bundle.js",
  },
  plugins: [
    new webpack.ProvidePlugin({
      React: "react",
    }),
    new webpack.HotModuleReplacementPlugin(),
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
};
```

> prod, dev 모드 둘다 공통으로 사용되는 설정이며,
> 각 요소는 다음과 같은 설정을 의미한다.

> - entry : 처음 실행되는 기본 시작 파일
> - resolve : 확장자나 경로를 처리하기 위해 설정하는 옵션
> - module : ts-loader, babel-loader를 설정하는 부분이다.

    - loader는 오른쪽에서 왼쪽 방향으로 적용되기에 ts-loader를 babel-loader보다 오른쪽에 위치시켜야 한다.

> - output : 번들화 된 파일을 export할 경로와 파일명을 설정하는 부분
> - plugins : 설치한 플러그인을 적용하는 옵션

</br>

#### webpack.dev.js

```javascript
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "development",
  devtool: "eval",
  devServer: {
    historyApiFallback: true,
    port: 3000,
    hot: true,
  },
});
```

</br>

#### webpack.prod.js

```javascript
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "production",
  devtool: "hidden-source-map",
});
```

> 공통적으로 쓰이는 webpack.commom.js를 import 한 후,
> webpack-merge를 사용하여
> webpack.dev.js, webpack.prod.js로 분리하여
> 원하는 webpack 설정을 환경에 맞게 추가할 수 있다.

각 요소는 다음과 같은 설정을 의미한다.

> - mode : 현재 어떤 모드로 동작하는 것인지를 확인

    - production : 배포 환경
    - development : 개발자 환경

> - devtool : 어떤 모드로 구조를 보여줄 것인지

    - eval : 최대 성능을 갖춘 개발 빌드를 위해 개발 환경에서 추천됨
    - hidden-source-map : 가장 느리지만 외부에서 리액트 구조를 확인할 수 없게하기에 배포 단계에서 추천됨

</br>

### 명령어 설정

#### package.json

```javascript
...
  "scripts": {
    "dev": "webpack-dev-server --config webpack.dev.js --open --hot",
    "build": "webpack --config webpack.prod.js",
    "start": "webpack --config webpack.dev.js"
  },
...
```

다음과 같은 실행 명령을 package.json에 추가한다.

> 해당 세팅을 한 후,
> 실행을 해보면 기본 React와 동일하게
> 동작하는 것을 확인할 수 있다.

</br>

# 📌 느낀점

하나씩 기능을 추가해나가는 과정이 재미있었다.

아직 모든 세팅을 마친것은 아니지만,
Webpack에 많은 기능들이 존재하며

잘만 활용한다면 효율적으로
작업 환경을 구축할 수 있을것 같다는 생각이 들었다.

### [테크 블로그에 그림과 함께 더 쉽게 풀어쓴 글](https://velog.io/@tnehd1998/CRA%EC%97%86%EC%9D%B4-ReactTypeScript-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)
