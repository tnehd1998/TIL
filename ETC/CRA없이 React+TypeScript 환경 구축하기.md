# ๐ ์์ํ๊ฒ๋ ๊ณ๊ธฐ

ํ๋ก์ ํธ๋ฅผ ์งํํจ์ ์์ด
CRA ์์ด React App์ ๋ง๋ค์ด๋ณด์๋
์ข์ ์์ด๋์ด๋ฅผ ๋ฃ๊ณ 

CRA๋ฅผ ์ฌ์ฉํ์ง ์๊ณ 
React + TypeScript ํ๊ฒฝ์ ๊ตฌ์ถํด๋ณด๊ธฐ๋ก ํ๋ค.

## ๐  ํ์ํ ์ค์ 

### React ์ค์ 

```
npm init -y
npm i react react-dom
```

> ํ๋ก์ ํธ๋ฅผ ๊ตฌ์ถํจ์ ์์ด react, react-dom ํจํค์ง๋ฅผ ๋จผ์  ์ค์นํ๋ค.

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

> React์ ๊ธฐ๋ณธ์ ์ผ๋ก ํ์ํ
> index.html, index.tsx, App.tsx ํ์ผ๋ค์ ์์ฑํ๋ค.

</br>

### TypeScript ์ค์ 

```
npm i -D typescript @types/react @types/react-dom
tsc --init
```

> TypeScript๋ฅผ ์ฌ์ฉํ๊ธฐ์,
> TypeScript ๊ด๋ จ ํจํค์ง๋ฅผ ์ค์นํ ํ
> ํ์์คํฌ๋ฆฝํธ ์ค์  ํ์ผ์ ์ด๊ธฐํํ๋ค.

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

> ์์ ์ด ์ํ๋ ์คํ์ผ๋๋ก ์ค์ ์ ํ๋ค.
> tsconfig.json์ ๊ธฐ๋ฅ๋ค์ ๋ฐ๋ก ์ ๋ฆฌ๋ฅผ ํ๊ธฐ์
> ํน์  ์ค์  ๋ฐ ๊ธฐ๋ฅ์ด ๊ถ๊ธํ๋ค๋ฉด ํด๋น ๊ธ์ ํ์ธํ๋ฉด ๋๋ค.

[tsconfig.json ์ ๋ฆฌ](https://velog.io/@tnehd1998/tsconfig.json-%EB%82%B4%EC%9A%A9-%EC%A0%95%EB%A6%AC)

</br>

### Babel ์ค์ 

๋ค์๊ณผ ๊ฐ์ ํจํค์ง๋ฅผ ์ค์นํ๋ค.

```javascript
npm i -D babel-loader @babel/core @babel/preset-env
npm i -D @babel/preset-react @babel/preset-typescript

```

๊ฐ ํจํค์ง๋ ๋ค์๊ณผ ๊ฐ์ ์ญํ ์ ์ํํ๋ค.

#### babel-loader, @babel/core, @babel/preset-env

> - ๋ณดํต ES6+๋ฌธ๋ฒ์ผ๋ก ์ฝ๋๋ฅผ ์์ฑํ๋ฉด IE ์น ๋ธ๋ผ์ฐ์ ์ ๊ฐ์ด ๊ตฌํ ๋ธ๋ผ์ฐ์ ์์๋
>   ์ ๋๋ก ์ง์์ด ๋์ง ์๋๋ค.
> - ๊ทธ๋ ๊ธฐ์ babel JavaScript ์ปดํ์ผ๋ฌ๋ฅผ ํ์ฉํ์ฌ
>   IE์์๋ ES6+ ๋ฌธ๋ฒ์ ์ฌ์ฉํ  ์ ์๊ฒ ํ๋ค.

</br>

#### @babel/preset-typescript

> - ์๋ ๋ฐ๋ฒจ๊ณผ ํ์์คํฌ๋ฆฝํธ๋ ๋ฐ๋ก ์์์ด ๋์์ง๋ง,
>   ํด๋น ํ๋ฌ๊ทธ์ธ์ ํตํด ํ์์คํฌ๋ฆฝํธ์ ๋ฐ๋ฒจ์ด ์กฐํ๋กญ๊ฒ ๋ณํฉํ์ฌ ์ฌ์ฉํ๊ฒ ๋๋ค.
> - TypeScript๋ฅผ ์ฌ์ฉํ๋ค๋ฉด ํ์ํ ํ๋ฌ๊ทธ์ธ๋ค์ ์งํฉ์ด๋ค.

</br>

#### @babel/preset-react

> - jsx๋ก ์์ฑ๋ ์ฝ๋๋ค์ createElement ํจ์๋ฅผ ์ด์ฉํ ์ฝ๋๋ก ๋ณํํด ์ฃผ๋
>   ๋ฐ๋ฒจ ํ๋ฌ๊ทธ์ธ์ด ๋ด์ฅ๋์ด ์์
> - React๋ฅผ ์ฌ์ฉํ๋ค๋ฉด ํ์ํ ํ๋ฌ๊ทธ์ธ๋ค์ ์งํฉ์ด๋ค.

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

> ํจํค์ง ์ค์น๋ฅผ ํ ํ,
> ํด๋น ์ฝ๋๋ฅผ
> ๋ฐ๋ฒจ ์ค์  ํ์ผ์ ์๋ฏธํ๋
> babel.config.js์ ์๋ ฅํ๋ค.

</br>

### Webpack ์ค์ 

๋ค์๊ณผ ๊ฐ์ ํจํค์ง๋ฅผ ์ค์นํ๋ค.

```
npm i -D webpack webpack-cli webpack-dev-server webpack-merge
npm i -D html-webpack-plugin ts-loader
```

๊ฐ ํจํค์ง๋ ๋ค์๊ณผ ๊ฐ์ ์ญํ ์ ์ํํ๋ค.

#### webpack, webpack-cli

> - webpack์ ์ฌ์ฉํ๊ธฐ ์ํด์ ๊ธฐ๋ณธ์ ์ผ๋ก ํ์ํ ํจํค์ง

    - webpack : ์นํฉ ๊ทธ ์์ฒด์ ํจํค์ง
    - webpack-cli : ํฐ๋ฏธ๋์์ webpack ์ปค๋งจ๋๋ฅผ ์คํํ  ์ ์๊ฒ ํด์ฃผ๋ ์ปค๋งจ๋๋ผ์ธ ๋๊ตฌ

</br>

#### webpack-dev-server

> - ๋น ๋ฅธ ์ค์๊ฐ ๋ฆฌ๋ก๋๋ฅผ ๊ฐ๋ฅํ๊ฒ ํ๋ ๊ฐ๋ฐ ์๋ฒ

    - ๋์คํฌ์ ์ ์ฅ๋์ง ์๋ ๋ฉ๋ชจ๋ฆฌ ์ปดํ์ผ์ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ ์ปดํ์ผ ์๋๊ฐ ๋น ๋ฆ
    - webpack.config.js์์ devServer ์ต์์ ํตํด ์ต์์ ์ง์ ํ์ฌ ์ฌ์ฉ์ด ๊ฐ๋ฅํจ

</br>

#### webpack-merge

> - webpack์ dev, prod ๋ชจ๋๋ก
>   ํธ์ํ๊ฒ ๋ถ๋ฆฌํ์ฌ ๊ตฌ์ถํ  ์ ์๊ฒ ๋์์ค๋ค.

</br>

#### html-webpack-plugin

> - html ํ์ผ์ ํํ๋ฆฟ์ผ๋ก ์์ฑํ  ์ ์๊ฒ ๋์์ฃผ๋ ํ๋ฌ๊ทธ์ธ

</br>

#### ts-loader

> - ํ์์คํฌ๋ฆฝํธ ์ฝ๋๋ฅผ ์๋ฐ์คํฌ๋ฆฝํธ ์ฝ๋๋ก ๋ณํ

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

> prod, dev ๋ชจ๋ ๋๋ค ๊ณตํต์ผ๋ก ์ฌ์ฉ๋๋ ์ค์ ์ด๋ฉฐ,
> ๊ฐ ์์๋ ๋ค์๊ณผ ๊ฐ์ ์ค์ ์ ์๋ฏธํ๋ค.

> - entry : ์ฒ์ ์คํ๋๋ ๊ธฐ๋ณธ ์์ ํ์ผ
> - resolve : ํ์ฅ์๋ ๊ฒฝ๋ก๋ฅผ ์ฒ๋ฆฌํ๊ธฐ ์ํด ์ค์ ํ๋ ์ต์
> - module : ts-loader, babel-loader๋ฅผ ์ค์ ํ๋ ๋ถ๋ถ์ด๋ค.

    - loader๋ ์ค๋ฅธ์ชฝ์์ ์ผ์ชฝ ๋ฐฉํฅ์ผ๋ก ์ ์ฉ๋๊ธฐ์ ts-loader๋ฅผ babel-loader๋ณด๋ค ์ค๋ฅธ์ชฝ์ ์์น์์ผ์ผ ํ๋ค.

> - output : ๋ฒ๋คํ ๋ ํ์ผ์ exportํ  ๊ฒฝ๋ก์ ํ์ผ๋ช์ ์ค์ ํ๋ ๋ถ๋ถ
> - plugins : ์ค์นํ ํ๋ฌ๊ทธ์ธ์ ์ ์ฉํ๋ ์ต์

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

> ๊ณตํต์ ์ผ๋ก ์ฐ์ด๋ webpack.commom.js๋ฅผ import ํ ํ,
> webpack-merge๋ฅผ ์ฌ์ฉํ์ฌ
> webpack.dev.js, webpack.prod.js๋ก ๋ถ๋ฆฌํ์ฌ
> ์ํ๋ webpack ์ค์ ์ ํ๊ฒฝ์ ๋ง๊ฒ ์ถ๊ฐํ  ์ ์๋ค.

๊ฐ ์์๋ ๋ค์๊ณผ ๊ฐ์ ์ค์ ์ ์๋ฏธํ๋ค.

> - mode : ํ์ฌ ์ด๋ค ๋ชจ๋๋ก ๋์ํ๋ ๊ฒ์ธ์ง๋ฅผ ํ์ธ

    - production : ๋ฐฐํฌ ํ๊ฒฝ
    - development : ๊ฐ๋ฐ์ ํ๊ฒฝ

> - devtool : ์ด๋ค ๋ชจ๋๋ก ๊ตฌ์กฐ๋ฅผ ๋ณด์ฌ์ค ๊ฒ์ธ์ง

    - eval : ์ต๋ ์ฑ๋ฅ์ ๊ฐ์ถ ๊ฐ๋ฐ ๋น๋๋ฅผ ์ํด ๊ฐ๋ฐ ํ๊ฒฝ์์ ์ถ์ฒ๋จ
    - hidden-source-map : ๊ฐ์ฅ ๋๋ฆฌ์ง๋ง ์ธ๋ถ์์ ๋ฆฌ์กํธ ๊ตฌ์กฐ๋ฅผ ํ์ธํ  ์ ์๊ฒํ๊ธฐ์ ๋ฐฐํฌ ๋จ๊ณ์์ ์ถ์ฒ๋จ

</br>

### ๋ช๋ น์ด ์ค์ 

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

๋ค์๊ณผ ๊ฐ์ ์คํ ๋ช๋ น์ package.json์ ์ถ๊ฐํ๋ค.

> ํด๋น ์ธํ์ ํ ํ,
> ์คํ์ ํด๋ณด๋ฉด ๊ธฐ๋ณธ React์ ๋์ผํ๊ฒ
> ๋์ํ๋ ๊ฒ์ ํ์ธํ  ์ ์๋ค.

</br>

# ๐ ๋๋์ 

ํ๋์ฉ ๊ธฐ๋ฅ์ ์ถ๊ฐํด๋๊ฐ๋ ๊ณผ์ ์ด ์ฌ๋ฏธ์์๋ค.

์์ง ๋ชจ๋  ์ธํ์ ๋ง์น๊ฒ์ ์๋์ง๋ง,
Webpack์ ๋ง์ ๊ธฐ๋ฅ๋ค์ด ์กด์ฌํ๋ฉฐ

์๋ง ํ์ฉํ๋ค๋ฉด ํจ์จ์ ์ผ๋ก
์์ ํ๊ฒฝ์ ๊ตฌ์ถํ  ์ ์์๊ฒ ๊ฐ๋ค๋ ์๊ฐ์ด ๋ค์๋ค.

### [ํํฌ ๋ธ๋ก๊ทธ์ ๊ทธ๋ฆผ๊ณผ ํจ๊ป ๋ ์ฝ๊ฒ ํ์ด์ด ๊ธ](https://velog.io/@tnehd1998/CRA%EC%97%86%EC%9D%B4-ReactTypeScript-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)
