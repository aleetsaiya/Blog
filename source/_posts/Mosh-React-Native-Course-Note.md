---
title: Mosh React Native Course Note
date: 2022-03-28 22:01:47
tags:
---

# Mosh - React Native 課程筆記

## 目錄

+ [Ch1. Getting Started](#ch1.-getting-started)
+ [Ch2. Fundamental Concepts](#ch2.-fundemental-concepts)
+ [Ch3. Layout](#ch3.-layout)

## Getting Started

### 建立 React Native 專案

```console
npm install -g expo-cli
expo init myApp
```

### Vscode - Extension
React Native Tools: 可以讓我們在 vscode 裡面 debug 我們的 App

### 使用模擬器

android emulator: 

1. 下載 android studio
2. 下載完後，從 Actions 中選擇 AVD (Android Virtual Device) Manager 
3. 選擇 Create Virtual Device
4. 選擇裝置 (建議選擇有 Play Store 的裝置)
5. 選擇裝置 OS (建議選擇第二個，為最新且穩定的版本)
6. 下載完後選擇剛剛下載的版本，然後命名

這樣就建立完成一個 android emulator 

### 執行 React Native Project (android emulator)

1. 在 cmd 中輸入: `npm start`
2. 在開啟 android emulator 後，於 cmd 選擇 _open android_

接下來就會在 android emulator 上下載並執行我們的
程式

### 使用實際手機測試 (android)

1. 從 App Store 下載 _Expo Go_
2. 在 cmd 中輸入: `npm start`
3. 用 _Expo Go_ 內建的 QR Code 掃描器，掃描螢幕上的 QR Code (手機跟電腦要用同個網路)

_(搖晃手機可以開啟 developer menu)_

### 將 APP 發布到網站上

1. 在 npm start 執行 App 後，可以從 cmd 點開 localhost 連結，開啟網頁
2. 從網頁內點 publish or republish project
3. 在 cmd 裡面登入 expo project 的帳號密碼

## Ch2. Fundamental Concepts

### Component
+ `<View/>`: 跟 div 差不多
+ `<SafeAreaView/>`:  排除掉會被手機擋住的地方
+ `<Text/>`
```jsx
<Text numberOfLines={1} onPress={handlePress}>Hello World!</Text>
```
+ `Image`
```jsx
// 使用內部圖案
<Image source={require('./assets/icon.png')}/>
// 使用外部圖案 - soucre 接收物件
<Image
  fadeDuration={100} // fade 100ms
  blurRadius={5}
  source={{
    width: 200,
    height: 300,
    uri: "https://picsum.photos/200/300",
  }}
/>
```
+ `<TouchableWithoutFeedback/>`, `<TouchableOpacity/>`, `<TouchableHighlight/>`: 讓圖片能夠點擊
```jsx
<TouchableHighlight onPress={() => console.log("Press!")}>
  <Image
    source={{
      width: 200,
      height: 300,
      uri: "https://picsum.photos/200/300",
    }}
  />
</TouchableHighlight>
```
+ `<Button/>`
```jsx
<Button
  color="orange"
  title="Click Me!"
  onPress={() => console.log("Clicked")}
/>
```
+ `Alert`: (function) 跳出詢問視窗 
```jsx
Alert.alert(
  "Alert title", 
  "message", 
  [
    // Alert 中的選項
    { text: "Yes", onPress: () => console.log("Yes") },
    { text: "No",  onPress: () => console.log("No")  },
  ]
)
```
+ `Platform`: (function) 偵測os
```jsx
alert("This Platform's OS is : " + Platform.OS)
```
## Ch3. Layout

+ 獲得裝置方向: 
```jsx
// 1. Dimentions.get("screen")
console.log(Dimentios.get("screen"))
// 2. use hooks (npm install @react-native-community/hooks)
import {
  useDimensions,
  useDeviceOrientation,
} from "@react-native-community/hooks";
// get orientation from hooks
const orientation = useDeviceOrientation();
const dimensions = useDimensions();
```

+ `flex` in style: `flex: 1` 代表全部, 0.5 代表一半