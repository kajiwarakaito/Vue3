## 「Vue3 フロントエンド開発の教科書」11章
## 11.1 Paniaの基本
### 11.1.3 ストアファイル内記述の基本構造
```ts
vueChapter11.1/pinia-basic/src/stores/counter.ts

import {defineStore} from 'pinia';

export const useCounterStore = defineStore({
  id: 'counter',
  state: () => ({
    counter: 0
  }),
  getters: {
    doubleCount: (state) => state.counter * 2
  },
  actions: {
    increment() {
      this.counter++
    }
  }
});
```
<br>

### 11.1.4 ステートとそのインターフェース
```ts
vueChapter11.1/pinia-basic/src/stores/counter.ts

import { defineStore } from "pinia";

interface State {
  counter: number;
}

export const useCounterStore = defineStore({
  id: "counter",
  state: (): State => {
    return {
      counter: 0
    };
  },
  getters: {
    doubleCount: (state): number => {
      return state.counter * 2;
    }
  },
  actions: {
    incrementCount(): void {
      this.counter++;
    }
  }
});
```
<br>

### 11.1.6 コンポーネントからのストア利用
```ts
vueChapter11.1/pinia-basic/src/App.vue

<script setup lang="ts">
  import { computed } from 'vue';
  import { useCounterStore } from './stores/counter';

  const counterStore = useCounterStore();
  const count = computed(
    (): number => {
      return counterStore.counter;
    }
  );
  const doubleCount = computed(
    (): number => {
      return counterStore.doubleCount;
    }
  );
  const onIncrementClick = () => {
    counterStore.incrementCount();
  };
</script>

<template>
  <p>現在のポイント：{{ count }}</p>
  <p>現在のポイントの更に倍{{ doubleCount }}</p>
  <button v-on:click="onIncrementClick">加算</button>
</template>
```
<br>

## 11.2 引数を使う一歩進んだPiniaの利用方法
### 11.2.2 Provideの代わりの初期リストの用意
```ts
vueChapter11.2/pinia-fundamental/src/stores/members.ts

import { defineStore } from "pinia";
import type { Member } from "@/interfaces";

interface State {
    memberList: Map<number, Member>;
}

export const useMembersStore = defineStore({
    id: "members",
    state: (): State => {
        return {
            memberList: new Map<number, Member>()
        };
    },
    getters: {
    },
    actions: {
        initList(): void {
            this.memberList.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com",
        points: 35, note: "初回入会特典あり"});
            this.memberList.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@example.com",
        points: 53});
        },
    }
});
```
<br>

```ts
vueChapter11.2/pinia-fundamental/src/App.vue

<script setup lang="ts">
  import { RouterView } from 'vue-router';
  import { useMembersStore } from "@/stores/members";

  const membersStore = useMembersStore();
  membersStore.initList();
</script>

<template>
  <header>
    <h1>Piniaサンプル</h1>
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
  #breadcrumbs ul li:before {
    content: " > ";
  }
  #breadcrumbs ul li:first-child:before {
    content: none;
  }
</style>
```
<br>

### 11.2.3 Injectの代わりにリストデータをストアから取得
```ts
vueChapter11.2/pinia-fundamental/src/views/member/MemberList.ts

<script setup lang="ts">
    import { computed } from 'vue';
    import { RouterLink } from 'vue-router';
    import type { Member } from '@/interfaces';
    import { useMembersStore } from '@/stores/members';

    const membersStore = useMembersStore();

    const memberList = computed(
        (): Map<number, Member> => {
            return membersStore.memberList;
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

### 11.2.4 Provideの代わりの初期リストの用意
```ts
vueChapter11.2/pinia-fundamental/src/views/member/MemberDetail.vue

<script setup lang="ts">
    import { computed } from 'vue';
    import { RouterLink } from 'vue-router';
    import type { Member } from '@/interfaces';
    import { useMembersStore } from '@/stores/members';

    interface Props {
        id: number;
    }
    const props = defineProps<Props>();

    const membersStore = useMembersStore();
    const member = computed(
        (): Member => {
            return membersStore.getById(props.id);
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

### 11.2.5 引数付きのゲッタの定義
```ts
vueChapter11.2/pinia-fundamental/src/stores/members.ts

import { defineStore } from "pinia";
import type { Member } from "@/interfaces";

interface State {
    memberList: Map<number, Member>;
}

export const useMembersStore = defineStore({
    id: "members",
    state: (): State => {
        return {
            memberList: new Map<number, Member>()
        };
    },
    getters: {
        getById: (state) => {
            return (id: number): Member => {
                const member = state.memberList.get(id) as Member;
                return member;
            };
        }
    },
    actions: {
        initList(): void {
            this.memberList.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com",
        points: 35, note: "初回入会特典あり"});
            this.memberList.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@example.com",
        points: 53});
        },
    }
});
```
<br>

### 11.2.6 引数付きのアクションの利用
```ts
vueChapter11.2/pinia-fundamental/src/views/member/MemberAdd.vue

<script setup lang="ts">
    import { reactive } from 'vue';
    import { RouterLink, useRouter } from "vue-router";
    import type { Member } from "@/interfaces";
    import { useMembersStore } from '@/stores/members';

    const router = useRouter();
    const membersStore = useMembersStore();
    const member: Member = reactive({
            id: 0,
            name: "",
            email: "",
            points: 0,
            note: ""
        });
    const onAdd = (): void => {
        membersStore.addMember(member);
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

```ts
vueChapter11.2/pinia-fundamental/src/stores/members.ts

import { defineStore } from "pinia";
import type { Member } from "@/interfaces";

interface State {
    memberList: Map<number, Member>;
}

export const useMembersStore = defineStore({
    id: "members",
    state: (): State => {
        return {
            memberList: new Map<number, Member>()
        };
    },
    getters: {
        getById: (state) => {
            return (id: number): Member => {
                const member = state.memberList.get(id) as Member;
                return member;
            };
        }
    },
    actions: {
        initList(): void {
            this.memberList.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com",
        points: 35, note: "初回入会特典あり"});
            this.memberList.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@example.com",
        points: 53});
        },
        addMember(member: Member): void {
            this.memberList.set(member.id, member);
        }
    }
});
```
<br>

## 11.3 アクションの応用
### 11.3.1 外部連携もアクションの役割
```ts
vueChapter11.3/pinia-storage/src/App.vue

<template>
  <header>
    <h1>Piniaサンプル</h1>
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
  #breadcrumbs ul li:before {
    content: " > ";
  }
  #breadcrumbs ul li:first-child:before {
    content: none;
  }
</style>
```
<br>

### 11.3.2 セッションストレージからデータの取得
```ts
vueChapter11.3/pinia-storage/src/stores/members.ts

import { defineStore } from "pinia";
import type { Member } from "@/interfaces";

interface State {
    memberList: Map<number, Member>;
}

export const useMembersStore = defineStore({
    id: "members",
    state: (): State => {
        return {
            memberList: new Map<number, Member>()
        };
    },
    getters: {
        getById: (state) => {
            return (id: number): Member => {
                const member = state.memberList.get(id) as Member;
                return member;
            };
        },
    },
    actions: {
        prepareMemberList(): void {
            let memberList = new Map<number, Member>();
            const memberListJSONStr = sessionStorage.getItem("memberList");
            if(memberListJSONStr != undefined) {
                const memberListJSON = JSON.parse(memberListJSONStr);
                memberList = new Map<number, Member>(memberListJSON);
            }
            this.memberList = memberList;
        },
    }
});
```
<br>

### 11.3.4 MemberList.vueの変更
```ts
vueChapter11.3/pinia-storage/src/views/member/MemberList.vue

<script setup lang="ts">
    import { computed } from 'vue';
    import { RouterLink } from 'vue-router';
    import type { Member } from '@/interfaces';
    import { useMembersStore } from '@/stores/members';

    const membersStore = useMembersStore();
    membersStore.prepareMemberList();

    const memberList = computed(
        (): Map<number, Member> => {
            return membersStore.memberList;
        }
    );
    const isEmptyList = computed(
        (): boolean => {
            return membersStore.isMemberListEmpty;
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
                <li v-if="isEmptyList">会員情報は存在しません</li>
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
vueChapter11.3/pinia-storage/src/stores/members.ts

import { defineStore } from "pinia";
import type { Member } from "@/interfaces";

interface State {
    memberList: Map<number, Member>;
}

export const useMembersStore = defineStore({
    id: "members",
    state: (): State => {
        return {
            memberList: new Map<number, Member>()
        };
    },
    getters: {
        getById: (state) => {
            return (id: number): Member => {
                const member = state.memberList.get(id) as Member;
                return member;
            };
        },
        isMemberListEmpty: (state): boolean => {
            return state.memberList.size == 0;
        }
    },
    actions: {
        prepareMemberList(): void {
            let memberList = new Map<number, Member>();
            const memberListJSONStr = sessionStorage.getItem("memberList");
            if(memberListJSONStr != undefined) {
                const memberListJSON = JSON.parse(memberListJSONStr);
                memberList = new Map<number, Member>(memberListJSON);
            }
            this.memberList = memberList;
        },
    }
});
```
<br>

### 11.3.5 Mapオブジェクトをセッションストレージに格納する方法
```ts
vueChapter11.3/pinia-storage/src/stores/members.ts

import { defineStore } from "pinia";
import type { Member } from "@/interfaces";

interface State {
    memberList: Map<number, Member>;
}

export const useMembersStore = defineStore({
    id: "members",
    state: (): State => {
        return {
            memberList: new Map<number, Member>()
        };
    },
    getters: {
        getById: (state) => {
            return (id: number): Member => {
                const member = state.memberList.get(id) as Member;
                return member;
            };
        },
        isMemberListEmpty: (state): boolean => {
            return state.memberList.size == 0;
        }
    },
    actions: {
        prepareMemberList(): void {
            let memberList = new Map<number, Member>();
            const memberListJSONStr = sessionStorage.getItem("memberList");
            if(memberListJSONStr != undefined) {
                const memberListJSON = JSON.parse(memberListJSONStr);
                memberList = new Map<number, Member>(memberListJSON);
            }
            this.memberList = memberList;
        },
        insertMember(member: Member): void {
            this.memberList.set(member.id, member);
            const memberListJSONStr = JSON.stringify([...this.memberList]);
            sessionStorage.setItem("memberList", memberListJSONStr);
        }
    }
});
```
<br>

```ts
vueChapter11.3/pinia-storage/src/views/member/MemberAdd.vue

<script setup lang="ts">
    import { reactive } from 'vue';
    import { RouterLink, useRouter } from "vue-router";
    import type { Member } from "@/interfaces";
    import { useMembersStore } from '@/stores/members';

    const router = useRouter();
    const membersStore = useMembersStore();
    const member: Member = reactive({
            id: 0,
            name: "",
            email: "",
            points: 0,
            note: ""
        });
    const onAdd = (): void => {
        membersStore.insertMember(member);
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
