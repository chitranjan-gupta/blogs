---
tags:
  - node
  - npm
  - react
  - react-native
  - reactnative
  - expo
  - react-native-devtools
  - debugging
  - debugging-react-native
  - debugging-expo-react-native
description: Debugging Expo React Native
slug: expo-react-native-devtools-debugging
published: "true"
created: 2025-01-26T14:48:00
updated: 2025-01-26T14:48:00
categories:
  - React
  - Programming
  - React Native
  - Expo
  - React Native Devtools
  - Debugging React Native
title: Debugging Expo React Native
---
[![react-native-devtools.png](https://reactnative.dev/assets/images/debugging-rndt-welcome-ac9602807bddf2752fc2a73c57028122.jpg)](https://reactnative.dev/assets/images/debugging-rndt-welcome-ac9602807bddf2752fc2a73c57028122.jpg)
### Introduction:
Debugging is an essential part of the development process, especially when working with complex frameworks like React Native. Fortunately, the React Native team has created a powerful debugging tool called React Native DevTools, designed to make debugging a more seamless and efficient experience.
### What is React Native DevTools?
React Native DevTools is a modern debugging experience for React Native, built from the ground up to be fundamentally integrated, correct, and reliable. It's specifically designed for debugging React app concerns and not to replace native tools. If you're working with native modules, you should continue using the debugging tools available in Android Studio and Xcode.
### Key Features:
React Native DevTools is based on the Chrome DevTools frontend, offering a familiar interface to web developers. Let's explore some of its core features:
1. **Console**: The Console panel allows you to view and filter messages, evaluate JavaScript, inspect object properties, and more.
2. **Sources & breakpoints**: The Sources panel enables you to view the source files in your app and register breakpoints. Breakpoints allow you to inspect the live state of the program and incrementally step through code.
3. **Memory**: The Memory panel allows you to take a heap snapshot and view the memory usage of your JavaScript code over time.
4. **React DevTools**: The Components and Profiler panels from the React DevTools browser extension are integrated into React Native DevTools, enabling you to inspect and update the rendered React component tree, highlight re-renders, and record performance profiles.
### How to open React Native Devtools Locally ?
 
 Here are the steps to open React Native DevTools:

1. Start your project with the command `npx expo start` in your terminal.
2. Press the `j` key in the terminal to open the debugger in Google Chrome or Microsoft Edge.
3. Alternatively, If using development builds or Expo Go, Shake the device and press the "Open JS Debugger" option in the developer menu.
4. Make sure your app is connected to the development server and try reloading the app if you encounter issues.
### How to open React Native Devtools through tunneling ?

[![Expo React Native Devtools](https://img.youtube.com/vi/Drkw2UwI1l0/0.jpg)](https://www.youtube.com/watch?v=Drkw2UwI1l0)

### Method 1

#### Edge Browser
- Open Edge Browser and navigate to https://microsoftedge.microsoft.com/addons/detail/expo-react-native-devtool/aakbhgdpbgkgdijllkpoaolnejhobjkl
- Click on Get to install the extension
- After that follow the steps from Step no 4  

To start React Native DevTools through tunneling, follow these steps:
1. Download the chrome extension zip from https://github.com/chitranjan-gupta/expo-react-native-devtools
2. Extract it and Open Chrome Based Browser and navigate to `chrome://extensions`
3. Enable Developer Mode and then click on Load Unpacked and choose the location where you have extracted the extension
4. After Installing the extension. Go to your expo project and start your project with the command `npx expo start --tunnel --go`.
5. You should start with `--go` mode because websocket tunneling does not work through `https` therefore `--go` start on `http`
6. Suppose your tunnel url `http://jk9w8fg-anonymous-8081.exp.direct`.
7. Click on the Icon of Expo React Native Devtools Extension and paste `http://jk9w8fg-anonymous-8081.exp.direct` and Click on Open Expo React Native Devtool
8. It will open a window or tab with React Native Devtool
### Method 2

To start React Native DevTools through tunneling, follow these steps:

1. Start your project with the command `npx expo start --tunnel --go`.
2. You should start with `--go` mode because websocket tunneling does not work through `https` therefore `--go` start on `http`
3. Suppose your tunnel url `http://jk9w8fg-anonymous-8081.exp.direct`.
4. Add this url exception list in chrome unsecure content otherwise chrome will redirect it to https.
5. then navigate to `http://jk9w8fg-anonymous-8081.exp.direct/json/list`, it should give json response of connected devices. If it is empty array connect it to device and then refresh it.
6.  It should look like this, select first `devtoolsFrontendUrl`.
```json
[
  {
    "id": "cb824cb576c4cdca4be753731dde1ec809e5794a-1",
    "title": "React Native Bridgeless [C++ connection]",
    "description": "com.chitranjangupta.shikshasetu.development",
    "type": "node",
    "devtoolsFrontendUrl": "devtools://devtools/bundled/js_app.html?experiments=true&v8only=true&ws=10.0.5.2%3A8081%2Finspector%2Fdebug%3Fdevice%3Dcb824cb576c4cdca4be753731dde1ec809e5794a%26page%3D1",
    "webSocketDebuggerUrl": "ws://10.0.5.2:8081/inspector/debug?device=cb824cb576c4cdca4be753731dde1ec809e5794a&page=1",
    "deviceName": "21091116UI - 13 - API 33",
    "reactNative": {
      "logicalDeviceId": "cb824cb576c4cdca4be753731dde1ec809e5794a",
      "capabilities": {
        "prefersFuseboxFrontend": true,
        "nativeSourceCodeFetching": false,
        "nativePageReloads": true
      }
    }
  },
  {
    "id": "cb824cb576c4cdca4be753731dde1ec809e5794a-2",
    "title": "React Native Bridgeless [C++ connection]",
    "description": "com.chitranjangupta.shikshasetu.development",
    "type": "node",
    "devtoolsFrontendUrl": "devtools://devtools/bundled/js_app.html?experiments=true&v8only=true&ws=10.0.5.2%3A8081%2Finspector%2Fdebug%3Fdevice%3Dcb824cb576c4cdca4be753731dde1ec809e5794a%26page%3D2",
    "webSocketDebuggerUrl": "ws://10.0.5.2:8081/inspector/debug?device=cb824cb576c4cdca4be753731dde1ec809e5794a&page=2",
    "deviceName": "21091116UI - 13 - API 33",
    "reactNative": {
      "logicalDeviceId": "cb824cb576c4cdca4be753731dde1ec809e5794a",
      "capabilities": {
        "prefersFuseboxFrontend": true,
        "nativeSourceCodeFetching": false,
        "nativePageReloads": true
      }
    }
  },
  {
    "id": "cb824cb576c4cdca4be753731dde1ec809e5794a-3",
    "title": "Reanimated UI runtime [C++ connection]",
    "description": "com.chitranjangupta.shikshasetu.development",
    "type": "node",
    "devtoolsFrontendUrl": "devtools://devtools/bundled/js_app.html?experiments=true&v8only=true&ws=10.0.5.2%3A8081%2Finspector%2Fdebug%3Fdevice%3Dcb824cb576c4cdca4be753731dde1ec809e5794a%26page%3D3",
    "webSocketDebuggerUrl": "ws://10.0.5.2:8081/inspector/debug?device=cb824cb576c4cdca4be753731dde1ec809e5794a&page=3",
    "deviceName": "21091116UI - 13 - API 33",
    "reactNative": {
      "logicalDeviceId": "cb824cb576c4cdca4be753731dde1ec809e5794a",
      "capabilities": {
        "prefersFuseboxFrontend": false,
        "nativeSourceCodeFetching": false,
        "nativePageReloads": false
      }
    }
  },
  {
    "id": "cb824cb576c4cdca4be753731dde1ec809e5794a-4",
    "title": "video-metadata-runtime [C++ connection]",
    "description": "com.chitranjangupta.shikshasetu.development",
    "type": "node",
    "devtoolsFrontendUrl": "devtools://devtools/bundled/js_app.html?experiments=true&v8only=true&ws=10.0.5.2%3A8081%2Finspector%2Fdebug%3Fdevice%3Dcb824cb576c4cdca4be753731dde1ec809e5794a%26page%3D4",
    "webSocketDebuggerUrl": "ws://10.0.5.2:8081/inspector/debug?device=cb824cb576c4cdca4be753731dde1ec809e5794a&page=4",
    "deviceName": "21091116UI - 13 - API 33",
    "reactNative": {
      "logicalDeviceId": "cb824cb576c4cdca4be753731dde1ec809e5794a",
      "capabilities": {
        "prefersFuseboxFrontend": false,
        "nativeSourceCodeFetching": false,
        "nativePageReloads": false
      }
    }
  }
]
```
8. Replace `devtools://devtools/bundled/js_app.html` with `http://jk9w8fg-anonymous-8081.exp.direct/debugger-frontend/rn_fusebox.html` and `ws=10.0.5.2%3A8081` with `ws=jk9w8fg-anonymous-8081.exp.direct` 
9. After replacing it will look like this `http://jk9w8fg-anonymous-8081.exp.direct/debugger-frontend/rn_fusebox.html?experiments=true&v8only=true&ws=jk9w8fg-anonymous-8081.exp.direct%2Finspector%2Fdebug%3Fdevice%3Dcb824cb576c4cdca4be753731dde1ec809e5794a%26page%3D1`
10. Navigate to this url it will open React Native Devtool
11. Make sure your app is connected to the development server and try reloading the app if you encounter issues.

>Note that tunneling allows you to debug your app remotely, but it may result in slower performance and increased latency. It is recommended to use local debugging when possible.
### Conclusion:
React Native DevTools is a powerful debugging tool for React Native developers. Its familiar interface and rich features make debugging a more enjoyable and productive experience. By incorporating the best of Chrome DevTools and React DevTools, it offers a comprehensive debugging solution for React Native apps.
### References
1. https://reactnative.dev/docs/react-native-devtools
2. https://docs.expo.dev/guides/using-hermes/
3. https://github.com/react-native-community/discussions-and-proposals/discussions/819
4. https://github.com/expo/expo/issues/34370
5. https://github.com/expo/expo/issues/31287