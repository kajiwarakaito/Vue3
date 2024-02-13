## 「Vue3 フロントエンド開発の教科書」10章
## 10.2 Vue Routerの基本
### 10.2.1 Vue Routerのプロジェクト
```ts
vueChapter10.2/router-basic/src/router/index.ts

import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView
    },
    {
      path: '/about',
      name: 'about',
      // route level code-splitting
      // this generates a separate chunk (About.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import('../views/AboutView.vue')
    }
  ]
})

export default router
```
<br>

### 10.2.3 ルーティング表示領域の設定
```ts
vueChapter10.2/router-basic/src/App.vue

<script setup lang="ts">
  import { reactive, provide } from 'vue';
  import { RouterView } from "vue-router";
  import type { Member } from "@/interfaces";

  const memberList = new Map<number, Member>();
  memberList.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com", points: 35,
  note: "初回入会特典あり"});
  memberList.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@examlpe.com", points: 53});
  provide("memberList", reactive(memberList));
</script>

<template>
  <header>
    <h1>Vue Routerサンプル</h1>
  </header>
  <main>
    <RouterView/>
  </main>
</template>

<style>
  main {
    border: blue 1px solid;
    padding: 10px;
  }
  #breadcrumbs ul li {
    display: inline;
    list-style-type: none;
  }
  #breadcrumbs {
    margin-left: 0px;
  }
  #breadcrumbs ul {
    padding-left: 0px;
  }
  #breadcrumbs ul .current {
    color: red;
  }
  #breadcrumbs ul li:befor {
    content: " > ";
  }
  #breadcrumbs ul li:first-child:before {
    content: none;
  }
</style>
```
<br>

### 10.2.4 ルーティング設定の基本構文
```ts
vueChapter10.2/router-basic/src/views/AppTop.vue

<template>
    <h1>TOP</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>TOP</li>
        </ul>
    </nav>
    <section>
        <p>
          会員管理はこちら
        </p>
    </section>
</template>
```
<br>

```ts
vueChapter10.2/router-basic/src/router/index.ts

import { createRouter, createWebHistory } from "vue-router";
import type { RouteRecordRaw } from "vue-router";
import AppTop from "@/views/AppTop.vue";

const routeSettings: RouteRecordRaw[] = [
  {
    path: "/",
    name: "AppTop",
    component: AppTop
  },
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: routeSettings
});

export default router;
```
<br>

### 10.2.5 コンポーネントの動的インポート設定
```ts
vueChapter10.2/router-basic/src/router/index.ts

import { createRouter, createWebHistory } from "vue-router";
import type { RouteRecordRaw } from "vue-router";
import AppTop from "@/views/AppTop.vue";

const routeSettings: RouteRecordRaw[] = [
  {
    path: "/",
    name: "AppTop",
    component: AppTop
  },
  {
    path: "/member/memberList",
    name: "MemberList",
    component: () => {
      return import("@/views/member/MemberList.vue");
    }
  }
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: routeSettings
});

export default router;
```
<br>

### 10.2.6 ルーティングリンクの設定
```ts
vueChapter10.2/router-basic/src/views/member/MemberList.vue

<template>
    <h1>会員管理</h1>
    <section>
        <h2>会員リスト</h2>
    </section>
</template>
```
<br>

```ts
vueChapter10.2/router-basic/src/views/AppTop.vue

<script setup lang="ts">
    import { RouterLink } from "vue-router";
</script>
<template>
    <h1>TOP</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>TOP</li>
        </ul>
    </nav>
    <section>
        <p>
            <RouterLink v-bind:to="{name: 'MemberList'}">
                会員管理はこちら
            </RouterLink>
        </p>
    </section>
</template>
```
<br>

## 10.3 ルートパラメータ
### 10.3.1 ルートパラメータの設定
```ts
vueChapter10.3//src/components/.vue


```
<br>

```ts
vueChapter10.3//src/App.vue


```
<br>

## 10.4 スクリプトブロックでのルーティング制御
### 10.4.1 会員情報追加画面の作成
```ts
vueChapter10.4//src/components/.vue


```
<br>

```ts
vueChapter10.4//src/App.vue


```
<br>

## 10.5 Vue Routerのその他の機能
### 10.5.1 スクリプトブロックでのルーティング情報の取得
```ts
vueChapter10.5//src/components/.vue


```
<br>

```ts
vueChapter10.5//src/App.vue


```