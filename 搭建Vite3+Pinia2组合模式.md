# 一、什么是`Vite3+Pinia2`组合模式

> vite 是一个现代的前端构建工具和开发服务器，而 pinia 是 Vue.js 的状态管理库，类似于 Vuex 但更轻量且易于使用。

[Vite 官方中文文档](https://cn.vitejs.dev/)

* Vite最新的脚手架

> Vite 是一个面向现代浏览器和 Node.js 的构建工具，它使用原生 ES 模块导入来提供闪电般的冷启动。Vite 主要服务于 Vue.js 项目，但也支持 React 和其他框架。

* Pinia最新的状态管理

> Pinia 是 Vue.js 的状态管理库，它是 Vuex 的下一代。Pinia 提供了更简洁、更直观的 API，并且与 Vue 3 的 Composition API 结合得很好。Pinia 的设计初衷是尽可能简单直观，同时仍然提供 Vuex 带来的所有功能。

# 二、搭建第一个 Vite 项目

1. 打开命令行工具，创建`vite-pinia-study`项目

```shell
npm create vite@latest vite-pinia-study
```

* 是否要安装create-vite@5.2.3

```shell
Need to install the following packages:
  create-vite@5.2.3
Ok to proceed? (y) y
```

* 选择vue，空格进行`选中`，回车`确认`

```shell
? Select a framework: » - Use arrow-keys. Return to submit.
    Vanilla
>   Vue
    React
    Preact
    Lit
    Svelte
    Solid
    Qwik
    Others
```

* 选择一个变体，这里选择自定义

```shell
? Select a variant: » - Use arrow-keys. Return to submit.
    TypeScript
    JavaScript
>   Customize with create-vue ↗
    Nuxt ↗
```

* 需要安装create-vue这个npm包

```shell
Need to install the following packages:
  create-vue@3.10.3
Ok to proceed? (y) y
```

* 根据项目需要进行选择

```shell
Vue.js - The Progressive JavaScript Framework

√ 是否使用 TypeScript 语法？ ... 否 / 是  （否）
√ 是否启用 JSX 支持？ ... 否 / 是  （否）
√ 是否引入 Vue Router 进行单页面应用开发？ ... 否 / 是 （是）
√ 是否引入 Pinia 用于状态管理？ ... 否 / 是  （是）
√ 是否引入 Vitest 用于单元测试？ ... 否 / 是   （否）
√ 是否要引入一款端到端（End to End）测试工具？ » 不需要  （不需要）
√ 是否引入 ESLint 用于代码质量检测？ ... 否 / 是  是（是）
? 是否引入 Prettier 用于代码格式化？ » 否 / 是  否（否）
? 是否引入 Vue DevTools 7 扩展用于调试? (试验阶段) » 否 / 是  (否)
```

* 项目创建成功标志

```shell
项目初始化完成，可执行以下命令：
  cd vite-pinia-study
  npm install
  npm run dev
```

# 三、简单了解vite-pinia

1. 修改端口,`vite.config.js`

> 在 vite.config.js 中，将端口号定义为数字 8080 而不是字符串 "8080"。这样可以确保 Vite 服务器使用正确的端口配置。

```js
import { fileURLToPath, URL } from "node:url";

import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
    },
  },
  server: {
    port: "8080",
  },
```

2. App.vue引用stores下的counter.js

> 使用 Pinia 来管理状态，可以避免手动处理异步操作的同步问题。Pinia 的 actions 和 getters 是同步的，允许你直接在组件中进行状态更新和使用，这使得状态管理更加简洁和直观。

```
import { storeToRefs } from "pinia";
import { useCounterStore } from "./stores/counter";
const counterStore = useCounterStore();
const { count, doubleCount } = storeToRefs(counterStore);
<template>
  {{ count }}{{ doubleCount }}
</template>
```

* store中是同步进行的

```
const handleClick = () => {
  counterStore.increment();
};
<button @click="handleClick">点击</button>
```
