---
tags:
  - node
  - npm
  - react
  - nextjs
description: "Uncaught Error: Rendered fewer hooks than expected. This may be caused by an accidental early return statement in React or Next.js or React Native or Expo"
slug: rendered-fewer-hooks-than-expected
published: "true"
created: 2020-09-21T10:01:00
updated: 2024-09-23T11:40:00
categories:
  - React
  - Programming
---
```js
Uncaught Error: Rendered fewer hooks than expected. This may be caused by an accidental early return statement.
```
```ts
import { SplashScreen, Redirect, Stack } from "expo-router";
import { useCallback, useEffect } from "react";

import { useAuth } from "@/core/auth";
import { STATUS, useFirstTime } from "@/core/store/use-first";
import { useUser } from "@/core/store/user";

const Layout = () => {
  const getUser = useUser((state) => state.getUser);
  const authStatus = useAuth((state) => state.status);
  const isFirstTime = useFirstTime((state) => state.status);

  const hideSplash = useCallback(async () => {
    await SplashScreen.hideAsync();
  }, []);

  useEffect(() => {
    if (authStatus !== "idle") {
      setTimeout(() => {
        hideSplash();
      }, 1000);
    }
  }, [hideSplash, authStatus]);

  if (isFirstTime === STATUS.yes) {
    return <Redirect href="/(unauth)/onboarding" />;
  }

  if (authStatus === "signOut") {
    return <Redirect href="/(unauth)/sign-up" />;
  }

  useEffect(() => {
    if (authStatus === "signIn") {
      getUser();
    }
  }, [authStatus, getUser]);

  return (
    <Stack>
      <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
      <Stack.Screen name="(courses)" options={{ headerShown: false }} />
    </Stack>
  );
};

export default Layout;
```
The problem is that in the first render all react hooks are called but when the `authStatus` variable is changed to `signOut` the last `useEffect` is not called. So the solutions is move the `useEffect` before any condition.

```ts
import { SplashScreen, Redirect, Stack } from "expo-router";
import { useCallback, useEffect } from "react";

import { useAuth } from "@/core/auth";
import { STATUS, useFirstTime } from "@/core/store/use-first";
import { useUser } from "@/core/store/user";

const Layout = () => {
  const getUser = useUser((state) => state.getUser);
  const authStatus = useAuth((state) => state.status);
  const isFirstTime = useFirstTime((state) => state.status);

  const hideSplash = useCallback(async () => {
    await SplashScreen.hideAsync();
  }, []);

  useEffect(() => {
    if (authStatus !== "idle") {
      setTimeout(() => {
        hideSplash();
      }, 1000);
    }
  }, [hideSplash, authStatus]);

  useEffect(() => {
    if (authStatus === "signIn") {
      getUser();
    }
  }, [authStatus, getUser]);

  if (isFirstTime === STATUS.yes) {
    return <Redirect href="/(unauth)/onboarding" />;
  }

  if (authStatus === "signOut") {
    return <Redirect href="/(unauth)/sign-up" />;
  }

  return (
    <Stack>
      <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
      <Stack.Screen name="(courses)" options={{ headerShown: false }} />
    </Stack>
  );
};

export default Layout;
```
###  Reference:
1. https://stackoverflow.com/questions/53472795/uncaught-error-rendered-fewer-hooks-than-expected-this-may-be-caused-by-an-acc
2. https://legacy.reactjs.org/docs/hooks-rules.html
3. https://react.dev/reference/rules/rules-of-hooks
