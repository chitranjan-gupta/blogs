---
tags:
  - node
  - npm
  - react
  - react-native
  - reactnative
  - expo
description: Tunneling in Expo React Native
slug: expo-tunnel
published: "true"
created: 2024-11-11T11:51:00
updated: 2024-11-11T11:51:00
categories:
  - React
  - Programming
  - React Native
  - Expo
title: Expo Tunnel
---
> WAN - Wide Area Network i.e. Internet

## Running on Android over WAN using ngrok

1. Install ngrok on System (Linux)
2. Forward the port 8081 using ngrok `ngrok http 8081`
3. Add the forwarded url in the terminal `export EXPO_PACKAGER_PROXY_URL=https://a1cc-104-199-117-204.ngrok-free.app`
4. Run the Expo server `npx expo start`
5. Install Expo Go Android App
6. Paste the url `exp://a1cc-104-199-117-204.ngrok-free.app`

## Running on Android over WAN using Gitpod Workspace

1. Export port 8081 in PORTS section of terminal and make it PUBLIC
2. Add the url in the terminal `export EXPO_PACKAGER_PROXY_URL=https://8081-chitranjangupta-project-hmc2iztl9q7.ws-us108.gitpod.io`
3. Run the Expo server `npx expo start`
4. Install Expo Go Android App
5. Paste the url `exp://8081-chitranjangupta-project-hmc2iztl9q7.ws-us108.gitpod.io`

## Running on Android over WAN using Github Codespace

1. Export port 8081 in PORTS section of terminal and make it PUBLIC
2. Add the url in the terminal `export EXPO_PACKAGER_PROXY_URL=https://cuddly-fiesta-rrw5v9vw55vc5754-8081.app.github.dev`
3. Run the Expo server `npx expo start`
4. Install Expo Go Android App
5. Paste the url `exp://cuddly-fiesta-rrw5v9vw55vc5754-8081.app.github.dev`

## Running on Android over WAN using Expo Tunnel
1. Run this command `npx expo start --tunnel` this will ask you to install `@expo/ngrok` npm package in global scope 
2. Press `y` to install
3. Install Expo Go Android App
4. Paste url `exp://sfff545-newwd.exp.direct`

> The Fast Refresh does not work on https connection because the websocket ssl termination does not work. Therefore to use fast refresh we have to connect using http connection.
> The `npx expo start --tunnel` start an http tunnel so it works without any configuration but with `Expo Go` only when it starts in `development build` it starts an https connection. So workaround this problem given below is solution.

#### Follow this instruction for development build to enable fast refresh
1. Install the `ngrok` npm module globally `npm i -g ngrok`
2. Set the auth token for ngrok because it will not work without authtoken 
	`ngrok config add authtoken <TOKEN>`
3. We have to downgrade ngrok http connection because by default with https 
	`ngrok http --scheme=http 8081`
4. then Add the url in the terminal `export EXPO_PACKAGER_PROXY_URL=http://a1cc-104-199-117-204.ngrok-free.app`
5. Run the Expo server `npx expo start`
6. Install Expo Go Android App
7. Paste url `http://a1cc-104-199-117-204.ngrok-free.app`

Sources:
1. [expo-cli](https://docs.expo.dev/more/expo-cli/)
