## 「Vue3 フロントエンド開発の教科書」12章
## 12.2
### 12.2.1
```ts
vueChapter12.2/async-db/src/App.vue

```
<br>

```ts
vueChapter12.2/async-db/src/interfaces.ts

export interface Member {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
}
```
<br>

```ts
vueChapter12.2/async-db/src/App.vue

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

```ts
vueChapter12.2/async-db/src/router/index.ts

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
<br>

```ts
vueChapter12.2/async-db/src/stores/counters.ts

import { ref, computed } from 'vue'
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const doubleCount = computed(() => count.value * 2)
  function increment() {
    count.value++
  }

  return { count, doubleCount, increment }
})
```
<br>

```ts
vueChapter12.2/async-db/src/stores/members.ts

import { defineStore } from "pinia";
import type { Member } from "@/interfaces";

export interface State {
    memberList: Map<number, Member>;
    isLoading: boolean;
}

let _database: IDBDatabase;
async function getDatabase(): Promise<IDBDatabase> {
    const promise = new Promise<IDBDatabase>(
        (resolve, reject): void => {
            if(_database != undefined) {
                resolve(_database);
            }
            else{
                const request = window.indexedDB.open("asyncdb", 1);
                request.onupgradeneeded = (event) => {
                    const target = event.target as IDBRequest;
                    const database = target.result as IDBDatabase;
                    database.createObjectStore("members", {keyPath: "id"});
                };
                request.onsuccess = (event) => {
                    const target = event.target as IDBRequest;
                    _database = target.result as IDBDatabase
                    resolve(_database);
                };
                request.onerror = (event) => {
                    console.log("ERROR: DBをオープンできません。", event);
                    reject(new Error("ERROR: DBをオープンできません。"));
                };
            }

        }
    );
    return promise;
}

export const useMembersStore = defineStore({
    id: "members",
    state: (): State => {
        return {
            memberList: new Map<number, Member>(),
            isLoading: true                                                         //
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
        async prepareMemberList(): Promise<boolean> {
            const database = await getDatabase();
            const promise = new Promise<boolean>(
                (resolve, reject) => {
                    const transaction = database.transaction("members", "readonly");
                    const objectStore = transaction.objectStore("members");
                    const memberList = new Map<number, Member>();
                    const request = objectStore.openCursor();
                    request.onsuccess = (event) => {
                        const target = event.target as IDBRequest;
                        const cursor = target.result as IDBCursorWithValue;
                        if(cursor) {
                            const id = cursor.key as number;
                            const member = cursor.value as Member;
                            cursor.continue();
                        }
                    };
                    transaction.oncomplete = () => {
                        this.memberList = memberList;
                        this.isLoading = false;
                        resolve(true);
                    };
                    transaction.onerror = (event) => {
                        console.log("ERROR: データ取得に失敗", event);
                        reject(new Error("ERROR: データ取得に失敗"));
                    };
                }
            );
            return promise;
        },
        async insertMember(member: Member): Promise<boolean> {
            const memberAdd: Member = {
                ...member
            };
            const database = await getDatabase();
            const promise = new Promise<boolean>(
                (resolve, reject) => {
                    const transaction = database.transaction("members", "readwrite");
                    const objectStore = transaction.objectStore("members");
                    objectStore.put(memberAdd);
                    transaction.oncomplete = () => {
                        resolve(true);
                    };
                    transaction.onerror = (event) => {
                        console.log("ERROR: データ登録に失敗", event);
                        reject(new Error("ERROR: データ登録に失敗"));
                    };
                }
            );
            return promise;
        }
    }
});
```
<br>

```ts
vueChapter12.2/async-db/src/views/AppTop.vue

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

```ts
vueChapter12.2/async-db/src/views/MemberAdd.vue

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
    const onAdd = (): void => {                                         //
        const promise = membersStore.insertMember(member);
        promise.then(
            (result: boolean) => {
                if(result) {
                    router.push({name: "MemberList"});
                }
            }
        );
        promise.catch(
            (error) => {
                console.log("データ登録失敗", error);
            }
        );
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
vueChapter12.2/async-db/src/views/MemberDetail.vue

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

```ts
vueChapter12.2/async-db/src/views/MemberList.vue

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
    const isLoading = computed(
        (): boolean => {
            return membersStore.isLoading;
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
        <p v-if="isLoading">
            データ取得中………
        </p>
        <section v-else>
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

## 12.3
### 12.3.1
```ts
vueChapter12.3/async-web/src/App.vue

<template>
  <header>
    <h1>Webアクセスサンプル</h1>
  </header>
  <main>
    <RouterView/>
  </main>
</template>
```
<br>

```ts
vueChapter12.3/async-web/src/router/index.ts

import type { RouteRecordRaw } from "vue-router";
import { createRouter, createWebHistory } from "vue-router";
import CityList from "@/views/CityList.vue";

const routeSettings: RouteRecordRaw[] = [
  {
    path: "/",
    name: "CityList",
    component: CityList
  },
  {
    path: "/weatherInfo/:id",
    name: "WeatherInfo",
    component: () => {
      return import("@/views/WeatherInfo.vue");
    },
    props: true
  },
]

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: routeSettings
})

export default router
```
<br>

```ts
vueChapter12.3/async-web/src/stores/weather.ts

import { defineStore } from "pinia";

export interface City {
    name: string;
    q: string;
}

interface State {
    cityList: Map<string, City>;
    selectedCity: City;
    isLoading: boolean;
    weatherDescription: string;
}

export const useWeatherStore = defineStore({
    id: "weather",
    state: (): State => {
        return {
            cityList: new Map<string, City>(),
            selectedCity: {
                name: "",
                q: ""
            },
            isLoading: true,
            weatherDescription: ""
        };
    },
    getters: {
    },
    actions: {
        prepareCityList() {
            this.cityList.set("Osaka",
            {
                name: "大阪",
                q: "Osaka"
            });
            this.cityList.set("Kobe",
            {
                name: "神戸",
                q: "Kobe"
            });
            this.cityList.set("Himeji",
            {
                name: "姫路",
                q: "Himeji"
            });
        },
        async recieveWeatherInfo(id: string) {
            this.selectedCity = this.cityList.get(id) as City;
            const weatherInfoUrl = "http://api.openweathermap.org/data/2.5/weather";
            const params:{
                lang: string,
                q: string,
                appId: string
            } =
            {
                lang: "ja",
                q: this.selectedCity.q,
                appId: "a6c34c4770b3a9638662a6e5041a55fc"
            }
            const queryParams = new URLSearchParams(params);
            const urlFull = `${weatherInfoUrl}?${queryParams}`;
            const response = await fetch(urlFull);
            const weatherInfoJSON = await response.json();
            const weatherArray = weatherInfoJSON.weather;
            const weather = weatherArray[0];
            this.weatherDescription = weather.description;
            this.isLoading = false;
        }
    }
});
```
<br>

```ts
vueChapter12.3/async-web/src/stores/counter.ts

import { ref, computed } from 'vue'
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const doubleCount = computed(() => count.value * 2)
  function increment() {
    count.value++
  }

  return { count, doubleCount, increment }
})
```
<br>

```ts
vueChapter12.3/async-web/src/views/CityList.vue

<script setup lang="ts">
    import { computed } from 'vue';
    import {RouterLink} from "vue-router";
    import {useWeatherStore} from "@/stores/weather";
    import type {City} from "@/stores/weather";

    const weatherStore = useWeatherStore();
    weatherStore.prepareCityList();

    const cityList = computed(
        (): Map<string, City> => {
            return weatherStore.cityList;
        }
    );
</script>

<template>
    <section>
        <h2>都市リスト</h2>
        <ul>
            <li
                v-for="[id, city] in cityList"
                v-bind:key="id">
                <RouterLink v-bind:to="{name: 'WeatherInfo', params: {id: id}}">
                    {{ city.name }}の天気
                </RouterLink>
            </li>
        </ul>
    </section>
</template>
```
<br>

```ts
vueChapter12.3/async-web/src/views/WeatherInfo.vue

<script setup lang="ts">
    import { computed } from 'vue';
    import { RouterLink } from 'vue-router';
    import { useWeatherStore } from '@/stores/weather';
    import type { City } from '@/stores/weather';

    interface Props {
        id: string;
    }
    const props = defineProps<Props>();

    const weatherStore = useWeatherStore();
    weatherStore.recieveWeatherInfo(props.id);
    const isLoading = computed(
        (): boolean => {
            return weatherStore.isLoading;
        }
    );
    const selectedCity = computed(
        (): City => {
            return weatherStore.selectedCity;
        }
    );
    const weatherDescription = computed(
        (): string => {
            return weatherStore.weatherDescription;
        }
    );
</script>

<template>
    <p v-if="isLoading">データ取得中……</p>
    <section v-else>
        <h2>{{ selectedCity.name }}の天気</h2>
        <p>{{ weatherDescription }}</p>
    </section>
    <p>
        リストに<RouterLink v-bind:to="{name: 'CityList'}">戻る</RouterLink>
    </p>
</template>
```
<br>

```ts
vueChapter12.3/async-web/src/App.vue

```