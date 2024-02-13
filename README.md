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
vueChapter10.3/router-basic/src/views/member/MemberList.vue

<script setup lang="ts">
    import { inject } from 'vue';
    import { RouterLink } from 'vue-router';
    import type { Member } from '@/interfaces';

    const memberList = inject("memberList") as Map<number, Member>;
</script>

<template>
    <h1>会員管理</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>
                <RouterLink v-bind:to="{name: 'AppTop'}">
                    TOP
                </RouterLink>
            </li>
            <li>会員リスト</li>
        </ul>
    </nav>
    <section>
        <h2>会員リスト</h2>
        <section>
            <ul>
                <li
                    v-for="[id, member] in memberList"
                    v-bind:key="id">
                    <RouterLink v-bind:to="{name: 'MemberDetail', params: {id: id}}">
                        IDが{{ id }}の{{ member.name }}さん
                    </RouterLink>
                </li>
            </ul>
        </section>
    </section>
</template>
```
<br>

```ts
vueChapter10.3/router-basic/src/views/member/MemberDetail.vue

<template>
    <h1>会員管理</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>
                <RouterLink v-bind:to="{name: 'AppTop'}">
                    TOP
                </RouterLink>
            </li>
            <li>
                <RouterLink v-bind:to="{name: 'MemberList'}">
                    会員リスト
                </RouterLink>
            </li>
            <li>会員詳細情報</li>
        </ul>
    </nav>
    <section>
        <h2>会員詳細情報</h2>
    </section>
</template>
```
<br>

```ts
vueChapter10.4/router-basic/src/router/index.ts

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
  },
  {
    path: "/member/detail/:id",
    name: "MemberDetail",
    component: () => {
      return import("@/views/member/MemberDetail.vue");
    },
  },
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: routeSettings
});

export default router;
```
<br>

### 10.3.3 ルートパラメータをPropsとして取得
```ts
vueChapter10.3/router-basic/src/views/member/MemberDetail.vue

<script setup lang="ts">
    import { inject, computed } from 'vue';
    import { RouterLink } from 'vue-router';
    import type { Member } from '@/interfaces';

    interface Props {
        id: number;
    }
    const props = defineProps<Props>();
    const memberList = inject("memberList") as Map<number, Member>;
    const member = computed(
        (): Member => {
            return memberList.get(props.id) as Member;
        }
    );
    const localNote = computed(
        (): string => {
            let localNote = "--";
            if(member.value.note != undefined) {
                localNote = member.value.note;
            }
            return localNote;
        }
    );
</script>

<template>
    <h1>会員管理</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>
                <RouterLink v-bind:to="{name: 'AppTop'}">
                    TOP
                </RouterLink>
            </li>
            <li>
                <RouterLink v-bind:to="{name: 'MemberList'}">
                    会員リスト
                </RouterLink>
            </li>
            <li>会員詳細情報</li>
        </ul>
    </nav>
    <section>
        <h2>会員詳細情報</h2>
        <dl>
            <dt>ID</dt>
            <dd>{{ member.id }}</dd>
            <dt>名前</dt>
            <dd>{{ member.name }}</dd>
            <dt>メールアドレス</dt>
            <dd>{{ member.email }}</dd>
            <dt>保有ポイント</dt>
            <dd>{{ member.points }}</dd>
            <dt>備考</dt>
            <dd>{{ localNote }}</dd>
        </dl>
    </section>
</template>
```
<br>

```ts
vueChapter10.3/router-basic/src/router/index.ts

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
  },
  {
    path: "/member/detail/:id",
    name: "MemberDetail",
    component: () => {
      return import("@/views/member/MemberDetail.vue");
    },
    props: (routes) => {
      const idNum = Number(routes.params.id);
      return {
        id:idNum
      };
    }
  },
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: routeSettings
});

export default router;
```
<br>

## 10.4 スクリプトブロックでのルーティング制御
### 10.4.1 会員情報追加画面の作成
```ts
vueChapter10.4/router-basic/src/views/member/MemberList.vue

<script setup lang="ts">
    import { inject } from 'vue';
    import { RouterLink } from 'vue-router';
    import type { Member } from '@/interfaces';

    const memberList = inject("memberList") as Map<number, Member>;
</script>

<template>
    <h1>会員管理</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>
                <RouterLink v-bind:to="{name: 'AppTop'}">
                    TOP
                </RouterLink>
            </li>
            <li>会員リスト</li>
        </ul>
    </nav>
    <section>
        <h2>会員リスト</h2>
        <p>
            新規登録は<RouterLink v-bind:to="{name: 'MemberAdd'}">こちら</RouterLink>から
        </p>
        <section>
            <ul>
                <li
                    v-for="[id, member] in memberList"
                    v-bind:key="id">
                    <RouterLink v-bind:to="{name: 'MemberDetail', params: {id: id}}">
                        IDが{{ id }}の{{ member.name }}さん
                    </RouterLink>
                </li>
            </ul>
        </section>
    </section>
</template>
```
<br>

```ts
vueChapter10.4/router-basic/src/router/index.ts

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
  },
  {
    path: "/member/detail/:id",
    name: "MemberDetail",
    component: () => {
      return import("@/views/member/MemberDetail.vue");
    },
    props: (routes) => {
      const idNum = Number(routes.params.id);
      return {
        id:idNum
      };
    }
  },
  {
    path: "/member/add",
    name: "MemberAdd",
    component: () => {
      return import("@/views/member/MemberAdd.vue");
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

```ts
vueChapter10.4/router-basic/src/views/member/MemberAdd.vue

<script setup lang="ts">
    import { inject, reactive } from 'vue';
    import { RouterLink } from "vue-router";
    import type { Member } from "@/interfaces";

    const memberList = inject("memberList") as Map<number, Member>;
    const member: Member = reactive(
        {
            id: 0,
            name: "",
            email: "",
            points: 0,
            note: ""
        }
    );
    const onAdd = (): void => {
        console.log(member);
    };
</script>
<template>
    <h1>会員管理</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>
                <RouterLink v-bind:to="{name: 'AppTop'}">
                    TOP
                </RouterLink>
            </li>
            <li>
                <RouterLink v-bind:to="{name: 'MemberList'}">
                    会員リスト
                </RouterLink>
            </li>
            <li>会員情報追加</li>
        </ul>
    </nav>
    <section>
        <h2>会員情報追加</h2>
        <p>
            情報を入力し、登録ボタンをクリックしてください
        </p>
        <form v-on:submit.prevent="onAdd">
            <dl>
                <dt>
                    <label for="addId">ID&nbsp;</label>
                </dt>
                <dd>
                    <input type="number" id="addId" v-model.number="member.id" required>
                </dd>
                <dt>
                    <label for="addName">名前&nbsp;</label>
                </dt>
                <dd>
                    <input type="text" id="addName" v-model="member.name" required>
                </dd>
                <dt>
                    <label for="addEmail">メールアドレス&nbsp;</label>
                </dt>
                <dd>
                    <input type="email" id="addEmail" v-model="member.email" required>
                </dd>
                <dt>
                    <label for="addPoints">保有ポイント&nbsp;</label>
                </dt>
                <dd>
                    <input type="number" id="addPoints" v-model.number="member.points" required>
                </dd>
                <dt>
                    <label for="addNote">備考&nbsp;</label>
                </dt>
                <dd>
                    <textarea id="addNote" v-model="member.note"></textarea>
                </dd>
            </dl>
            <button type="submit">登録</button>
        </form>
    </section>
</template>
```
<br>

### 10.4.2 ルーティングを制御するRouterオブジェクト
```ts
vueChapter10.4/router-basic/src/views/member/MemberAdd.vue

<script setup lang="ts">
    import { inject, reactive } from 'vue';
    import { RouterLink, useRouter } from "vue-router";
    import type { Member } from "@/interfaces";

    const router = useRouter();
    const memberList = inject("memberList") as Map<number, Member>;
    const member: Member = reactive(
        {
            id: 0,
            name: "",
            email: "",
            points: 0,
            note: ""
        }
    );
    const onAdd = (): void => {
        console.log(member);
        memberList.set(member.id, member);
        router.push({name: "MemberList"});
    };
</script>
<template>
    <h1>会員管理</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>
                <RouterLink v-bind:to="{name: 'AppTop'}">
                    TOP
                </RouterLink>
            </li>
            <li>
                <RouterLink v-bind:to="{name: 'MemberList'}">
                    会員リスト
                </RouterLink>
            </li>
            <li>会員情報追加</li>
        </ul>
    </nav>
    <section>
        <h2>会員情報追加</h2>
        <p>
            情報を入力し、登録ボタンをクリックしてください
        </p>
        <form v-on:submit.prevent="onAdd">
            <dl>
                <dt>
                    <label for="addId">ID&nbsp;</label>
                </dt>
                <dd>
                    <input type="number" id="addId" v-model.number="member.id" required>
                </dd>
                <dt>
                    <label for="addName">名前&nbsp;</label>
                </dt>
                <dd>
                    <input type="text" id="addName" v-model="member.name" required>
                </dd>
                <dt>
                    <label for="addEmail">メールアドレス&nbsp;</label>
                </dt>
                <dd>
                    <input type="email" id="addEmail" v-model="member.email" required>
                </dd>
                <dt>
                    <label for="addPoints">保有ポイント&nbsp;</label>
                </dt>
                <dd>
                    <input type="number" id="addPoints" v-model.number="member.points" required>
                </dd>
                <dt>
                    <label for="addNote">備考&nbsp;</label>
                </dt>
                <dd>
                    <textarea id="addNote" v-model="member.note"></textarea>
                </dd>
            </dl>
            <button type="submit">登録</button>
        </form>
    </section>
</template>
```
<br>

## 10.5 Vue Routerのその他の機能
### 10.5.3 ネストされたルーティング
```ts
vueChapter10.5/router-nested/src/views/member/MemberList.vue

<script setup lang="ts">
    import { inject } from 'vue';
    import { RouterLink } from 'vue-router';
    import type { Member } from '@/interfaces';

    const memberList = inject("memberList") as Map<number, Member>;
</script>

<template>
    <h1>会員管理</h1>
    <nav id="breadcrumbs">
        <ul>
            <li>
                <RouterLink v-bind:to="{name: 'AppTop'}">
                    TOP
                </RouterLink>
            </li>
            <li>会員リスト</li>
        </ul>
    </nav>
    <section>
        <h2>会員リスト</h2>
        <p>
            新規登録は<RouterLink v-bind:to="{name: 'MemberAdd'}">こちら</RouterLink>から
        </p>
        <section>
            <ul>
                <li
                    v-for="[id, member] in memberList"
                    v-bind:key="id">
                    <RouterLink v-bind:to="{name: 'MemberDetail', params: {id: id}}">
                        IDが{{ id }}の{{ member.name }}さん
                    </RouterLink>
                </li>
            </ul>
        </section>
        <RouterView/>
    </section>
</template>
```
<br>

```ts
vueChapter10.5/router-nested/src/router/index.ts

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
    },
    children: [
      {
        path: "detail/:id",
        name: "MemberDetail",
        component: () => {
          return import("@/views/member/MemberDetail.vue");
        },
        props: (routes) => {
          const idNum = Number(routes.params.id);
          return {
            id:idNum
          };
        }
      },
      {
        path: "add",
        name: "MemberAdd",
        component: () => {
          return import("@/views/member/MemberAdd.vue");
        }
      }
    ]
  },
  {
    path: "/member/detail/:id",
    name: "MemberDetail",
    component: () => {
      return import("@/views/member/MemberDetail.vue");
    },
    props: (routes) => {
      const idNum = Number(routes.params.id);
      return {
        id:idNum
      };
    }
  },
  {
    path: "/member/add",
    name: "MemberAdd",
    component: () => {
      return import("@/views/member/MemberAdd.vue");
    }
  },
  {
    path: "/:pathMatch(.*)*",
    name: "NotFound",
    component: () => {
      return import("@/views/NotFound.vue");
    }
  }
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: routeSettings
});

export default router;
```
このchildrenプロパティの追加にて教科書に書いてある例としては
```
http://localhost:xxxx/member/memberList/detail/33456
```
としてあるのだが、手元のプログラムでは
```
http://localhost:xxxx/member/detail/33456
```
となり、memberListが何故か省略されて表示された。  
<br>

### 10.5.4 マルチビュー
```ts
vueChapter10.5/router-multiple/src/App.vue

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
    <h1>マルチビューサンプル</h1>
  </header>
  <main>
    <RouterView/>
  </main>
  <section id="sub">
    <RouterView name="sub"/>
  </section>
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
  #breadcrumbs ul li:before {
    content: " > ";
  }
  #breadcrumbs ul li:first-child:before {
    content: none;
  }
</style>
```
<br>

```ts
vueChapter10.5/router-multiple/src/router/index.ts

import { createRouter, createWebHistory } from "vue-router";
import type { RouteRecordRaw } from "vue-router";
import AppTop from "@/views/AppTop.vue";

const routeSettings: RouteRecordRaw[] = [
  {
    path: "/",
    name: "AppTop",
    components: {
      default: AppTop,
      sub: Sub
    }
  },
  {
    path: "/member/memberList",
    name: "MemberList",
    component: () => {
      return import("@/views/member/MemberList.vue");
    }
  },
  {
    path: "/member/detail/:id",
    name: "MemberDetail",
    component: () => {
      return import("@/views/member/MemberDetail.vue");
    },
    props: (routes) => {
      const idNum = Number(routes.params.id);
      return {
        id:idNum
      };
    }
  },
  {
    path: "/member/add",
    name: "MemberAdd",
    component: () => {
      return import("@/views/member/MemberAdd.vue");
    }
  },
  {
    path: "/:pathMatch(.*)*",
    name: "NotFound",
    component: () => {
      return import("@/views/NotFound.vue");
    }
  }
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: routeSettings
});

export default router;
```
このプログラムに際しては
```
sub: Sub
```
の右のSubに対して**Subが見つかりません**というエラーが出ており、10.5.4の記述してあるプログラムコードの情報量が大変少なく、どのように解決すればよいか方法がわからなかった。  
おそらく前章の10.5.3までとは違い、このようなものもあるという例なのではないかと考えた。  