---
title: Use webpack to create react app with Typescript
date: 2022-04-13 00:24:26
tags:
---
1. 下載 React 17: 
```
npm install react@17 react-dom@17
```
2. 下載 typescript 及相關套件: 
```
npm install -D typescript @types/react @types/react-dom
```
3. 建立 `tsconfig.json`
```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react-jsx",
    "allowJs": true,
    "strict": true,
    "moduleResolution": "node",
    "sourceMap": true,
    "allowSyntheticDefaultImports": true
  },
  "include": ["./src/**/*"]
}
```
4. 建立 src folder，並在裡面建立 `env.d.ts`

> 不能命名為 `index.d.ts`，[參考連結](https://stackoverflow.com/questions/52759220/importing-images-in-typescript-react-cannot-find-module)

```js
// 讓 typescript 能夠將結尾是 png | jpg 的檔案視為 module
declare module "*.png";
declare module "*.jpg";
```
5. 下載 webpack: 
```
npm install -D webpack webpack-cli webpack-dev-server
```
6. 下載 webpack 相關套件: 
```
npm install -D style-loader css-loader html-webpack-plugin mini-css-extract-plugin
```
7. 下載 babel 相關套件: 
```
npm install -D babel-loader @babel/core @babel/preset-typescript @babel/preset-env @babel/preset-react @babel/plugin-transform-runtime
```
8. 建立 `.babelrc`
```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react", "@babel/preset-typescript"],
  "plugins": ["@babel/plugin-transform-runtime"]
}
```
9. 建立 `webpack.config.js`
```js
const prod = process.env.NODE_ENV === "production";
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const path = require("path");

module.exports = {
  mode: prod ? "production" : "development",
  entry: "./src/index.tsx",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "index.[hash].js",
  },
  module: {
    rules: [
      {
        test: /\.(jpe?g|png|svg)$/,
        type: "asset/resource",
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
      {
        test: /\.(ts|tsx)$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: "babel-loader",
        },
      },
    ],
  },
  devtool: "inline-source-map",
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
    new MiniCssExtractPlugin({
      filename: "index.[hash].css",
    }),
  ],
  resolve: {
    extensions: [".tsx", ".ts", ".jsx", ".js"],
  },
};
```
10 `package.json`
```json
"scripts": {
  "start": "webpack serve",
  "build": "set NODE_ENV=production&&webpack"
},
```
11. 加入 coding style 套件
```
npm install -D eslint prettier eslint-config-prettier
```
12. `npx eslint --init`
13. 在 `.eslintrc.js` 的 extends 內加入 prettier
```js
module.exports = {
  extends: ["prettier"],
  // 避免 import React 時錯誤
  rules: {
    "no-use-before-define": "off",
    "@typescript-eslint/no-use-before-define": ["error"],
  },
};
```