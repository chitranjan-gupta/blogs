---
tags:
  - node
  - npm
  - react
  - nextjs
  - tailwindcss
  - tailwind
description: "Syntax error: /workspaces/rappserver/frontend/styles/globals.css The `border-border` class does not exist. If `border-border` is a custom class, make sure it is defined within a `@layer` directive."
slug: the-border-border-class-does-not-exist
published: "true"
created: 2024-10-21T11:51:00
updated: 2024-10-21T11:51:00
categories:
  - React
  - Programming
  - Tailwindcss
title: The `border-border` class does not exist
---

[![tailwindcss-compile-error.png](https://i.postimg.cc/dtvbBsMf/tailwindcss-compile-error.png)](https://postimg.cc/rznZyLwG)
### Resolving Tailwind CSS Compilation Errors in Next.js

Integrating Tailwind CSS into your Next.js application can significantly enhance your styling capabilities. However, developers often encounter issues, especially in development mode, where Tailwind CSS fails to recognize certain classes. A common error is related to custom classes, such as `border-border`. In this post, we’ll dive deep into troubleshooting and fixing this error, ensuring a smooth development experience.
## Project Structure

- frontend 
	- pages 
	- styles 
		- globals.css 
	- next.config.js 
	- tailwind.config.js 
	- postcss.config.js 
- middleware
- node_modules
- index.js
- package.json
- package-lock.json

***Frontend is build using next.js pages router and using tailwind for UI.
Backend is using express and using next.js custom server to access the pages. When I build in production, the frontend works fine and tailwindcss compiles also but when I run in development mode the tailwindcss shows error: 
#### Common Compilation Error
The error you might encounter looks something like this:

```js
Syntax error: /workspaces/rappserver/frontend/styles/globals.css The `border-border` class does not exist. If `border-border` is a custom class, make sure it is defined within a `@layer` directive.

> 1 | @tailwind base;
 | ^
 2 | @tailwind components;
 3 | @tailwind utilities;
```
This indicates that Tailwind CSS cannot find the `border-border` class in your styles. Let’s explore how to resolve this issue step by step.

#### 1. Verify Your `tailwind.config.js`

The first step in troubleshooting is to ensure your Tailwind configuration is set up correctly. The `content` array in the `tailwind.config.js` file defines where Tailwind should look for class names. If any directories are missing, Tailwind won’t be able to find your classes.

Here’s an example configuration that covers typical paths in a Next.js project:
**this is tailwind.config.js file inside the frontend folder

```js
// frontend/tailwind.config.js
/** @type {import('tailwindcss').Config} */

module.exports = {
  darkMode: ["class"], // Enables dark mode with class strategy
  content: [
    "./app/**/*.{js,ts,jsx,tsx,mdx}",
    "./pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./components/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        border: "hsl(var(--border))",
        // Additional colors...
      },
    },
  },
  plugins: [require("tailwindcss-animate")], // Optional plugins
};

```

### To fix the error create `tailwind.config.js` with the following content inside the root folder.

> **Where your server is located.

```js
//tailwind.config.js
/** @type {import('tailwindcss').Config} */

module.exports = {
  darkMode: ["class"],
  content: [
    "./frontend/app/**/*.{js,ts,jsx,tsx,mdx}",
    "./frontend/pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./frontend/components/**/*.{js,ts,jsx,tsx,mdx}",
    "./frontend/src/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        border: "hsl(var(--border))",
        // Additional colors...
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
};
```

## References
1. https://nextjs.org/docs/pages/building-your-application/configuring/custom-server
