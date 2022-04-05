---
title: Typescript start
date: 2022-03-31 09:14:34
tags:
---

[布魯斯的 TypeScript 入門攻略｜輕鬆打造實時聊天室](https://hiskio.com/courses/628/about) 課程筆記

## Setup
1. `npm install -g typescript`
2. `tsc --init`
3. setting `ts.config`
```json
{
  // 我們寫的 index.ts 的資料夾
  "rootDir": "./src"
  // 設定輸出資料夾
  "outDir": "./dist"
  // 允許我們使用 js 檔
  "allowJS": true
  // 開啟這個選項能夠讓我們在 browser 的   console 中直接連結到是哪行 ts 檔輸出的，方便   debug
  "sourceMap": true
}
```
4. run `tsc` 
 
---------------------

## Setup webpack with ts-loader

因為 ts-loader 會使用 `tsc` 來幫我們處理 typescript 的檔案，所以要建立 `ts.config` 來設定 `tsc` 的一些參數。

docs: [Webpack Typescript](https://webpack.js.org/guides/typescript/)

1. `npm install typescript ts-loader`
2. 建立 `tsconfig.json`
```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "allowJs": true,
    "moduleResolution": "node",

    // path mapping
    // 讓我們可以使用其他符號代表路徑
    "baseUrl": "./src",
    "paths": {
      // @/* 代表 @/ 底下的東西會對應到 [baseUrl]/* (./src/)
      "@/*": ["*"]
    }
  }
}
```
3. 設定 `webpack.config.js`
```js
const path = require('path');

module.exports = {
  entry: './src/index.ts',
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    // webpack 默認只會編譯 js 檔，設定 resolve 讓 webpack 能夠將 tsx 以及 ts 也被 webpack 編譯
    extensions: ['.tsx', '.ts', '.js'],
    // 要使用 path mapping 的話，要加 alias 以免 webpack 看不懂
    // 設定 @ 對應到 src
    alias: {
      '@': path.resolve(__dirname, 'src'),
    }
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

> ts-loader uses tsc, the TypeScript compiler, and relies on your tsconfig.json configuration. Make sure to avoid setting module to "CommonJS", or webpack won't be able to **tree-shake** your code.

tree-shaking 能夠讓我們在 import 例如 `axios/get` 的時候不會將 `axios` 全部 import 近來，只會 import `get`，能夠有效地增加網站效能。 

---------------------

## Typescript 語法
### 斷言
斷言就是我們在一個變數後面加上 `as`，然後這個變數的型態就會變成我們設定的型態。

> 可以應用在當我們 fetch 一個 API 的時候。在 fetch 之前 Typescript 不會知道 fetch 完後的變數長什麼樣子，所以在 fetch 完後我們就可以自己用 `as` 來去設定它的型態

```js
async function getData() {
  const res = await fetch("https://jsonplaceholder.typicode.com/todos/1")   
  // fetch 完後設定資料型態
  const data = (await res.json()) as {
    userId: number;
    id: number;
    title: string;
    completed: boolean;
  };
  return data
}
```
### 強制斷言
有時候可能我們知道這個變數型態已經從原本的變成另一個型態，但是 Typescript 不給我們直接轉，像是底下的情況:
```js
let liveName2 = 91;
let liveName3 = liveName2 as string; //error -> Conversion of type 'number' to type 'string' may be a mistake
```
這時候就能使用強制斷言，先將變數型態轉成 `unknown`，再轉換成其它型態
```js
let liveName2 = 91;
let liveName3 = liveName2 as unknown as string;
```

### Never
never 就是 Typescript 判斷永遠不可能發生的類型，像是底下例子:
```js
let liveName: string | number;
liveName = "alee";
liveName = 9;

// never -> typeof liveName 不可能是 string
if (typeof liveName === "string") {
   liveName.split(""); //error 
}
```

### Type, Interface 擴充
使用 `type` 的話用 `&` 可以進行擴充，使用 `interface` 的話用 `extends` 可以進行擴充。
```js
// type
type Animal = {
  name: string;
};

type Dog = Animal & {
  age: number;
};

// interface
interface Animal2 {
  name: string;
}

interface Dog2 extends Animal {
  age: number;
}
```

> Type 與 Interface 差別在重複命名 Interface 的話會將原有的 Interface 進行擴充，而 Type 則無法重複命名。

### enum
設定每個符號代表的東西讓程式碼更容易閱讀
```js
enum LiveStatus {
  "streaming" = 0,
  "closed" = 1,
  "multiple" = 2,
}

let liveStatus = 0;
if (liveStatus === LiveStatus.streaming) {
  // ...
}

```

### Implement Interface
interface 可以用來約束建立出來的 class 必須需要有什麼功能以及屬性
```js
// 建立 interface
interface UserInterface {
  id: number;
  name: string;
  age: number;
  address: string;

  // 會員功能
  add: (data: any) => void;
  update: (id: number) => boolean;
  delete: (id: number) => boolean;
}

// implements interface
class LiveUser implements UserInterface {
  // 必須要有 interface 定義的東西
  id: number;
  name: string;
  age: number;
  address: string;

  add(data: any) {}
  update(id: number) {
    return true;
  }
  delete(id: number) {
    return true;
  }

  // 也可以新增額外的功能
  startLive() {}
  endLive() {}
}
```

### Abstract class
Abstract class 本身可以有自己的功能，也可以約束 `extends` 的 class 需要有什麼功能

```js
abstract class Animal {
  run() {
    console.log("run ...");
  }
  // 要求需要實作出 hello
  abstract hello(): void;
}

class Dog extends Animal {
  // 實作 hello
  hello() {
    console.log("Hello");
  }
}
```

### 泛型
基礎泛型就是打 `<>` 然後將我們期望的 type 傳進去
```js
function hello<T, U>(text: T, text2: U): T {
  console.log(text, text2);
  return text;
}

hello<number, string>(123, "alee");
hello<boolean, string>(true, "alee");
```
使用 interface 搭配泛型:
```js
interface Card<T> {
  title: string;
  desc: T;
}

function printCardInfo<T>(desc: T): Card<T> {
  const data: Card<T> = {
    title: "bruce",
    desc,
  };
  return data;
}
```
為了確保泛型內擁有特定的 property，可以使用 `extends` 來幫我們確認傳入的泛型型態是擁有這個 property 的。
ex.
```js
// 確保要傳入的型態含有 length
function logArrLen<T extends Array<number>>(arr: T) {
  console.log(arr.length);
}

logArrLen<Array<number>>([1, 2, 3]);
```

在泛型中使用 `infer` 的意思是: 我們先檢查條件有沒有達成，如果達成了的話就會透過 `infer` 幫我們建立一個參數，沒達成就不會透過 `infer` 建立參數，直接變成我們設定的 else 參數。
ex.
```js
// 檢查 T 是否有 extends Array，如果有的話建立參數 P (參數的值就是我們傳進去 Array 的值)
type Check<T> = T extends Array<infer P> ? P : never;

type Q = Check<[number]>; // type: number
type R = Check<["alee", 123]>; //type: 'alee', 123
type G = Check<string>; // type: never
```

### keyof
使用 `keyof` 可以獲得一個 type 的 keys 的 union。
ex.
```js
// keyof
interface UserCard {
  name: string;
  age: number;
  cardTitle: string;
  cardDesc: string;
}

// type: name | age | cardTitle | cardDesc
type UserCardKeys = keyof UserCard;

const key1: UserCardKeys = "name";
const key2: UserCardKeys = "cardTitle";
const key3: UserCardKeys = "cardDesc";

// 確保 K 是 T 的其中一個 key
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

### Map
```js
type UserData = {
  id: string;
  userName: string;
  roomName: string
}

export default class UserService {
  private userMap: Map<string, UserData>

  constructor() {
    this.userMap = new Map()
  }

  addUser(data: UserData) {
    this.userMap.set(data.id, data)
  }
}
```

## ES6 ES-Module vs CommonJS

+ CommonJS: 早期在開發 Node.js 的時候 Javascript 還沒有支援模組化的概念。使用 `require` 以及 `module.exports`
+ ES Module: ES6 新增的 Javascript 標準模組化方式。使用 `export` 以及 `import` 

```js
import "./index.css";

// CommonJS -> Nodejs 開發
const data = require("./commonJS");

// ES6 ES Module -> js標準
import { age, userName } from "./es6Module";
import * as all from "./es6Module";
```
