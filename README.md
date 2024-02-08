## 「Vue3 フロントエンド開発の教科書」8章  
## 8.1 子コンポーネントの利用
### 8.1.2 コンポーネントの作り方
```ts
vueChapter8.1/components-basics/src/components/OneSection.vue

<template>
  <section class="box">
    <h4>一つのコンポーネント</h4>
    <p>
      コンポーネントとは、
    </p>
  </section>
</template>

<style>
  .box {
    border: green 1px dashed;
    margin: 10px;
  }
</style>
```
<br>

### 8.1.3 子コンポーネントの利用方法
```ts
vueChapter8.1/components-basics/src/App.vue

<script setup lang="ts">
  import OnSection from "./components/OneSection.vue";
</script>

<template>
  <h1>コンポーネント基礎</h1>
  <section>
    <h2>コンポーネント1個</h2>
    <OnSection />
  </section>
  <section>
    <h2>コンポーネントが複数</h2>
    <OnSection />
    <OnSection />
    <OnSection />
  </section>
</template>

<style>
  section {
    border: blue 1px solid;
    margin: 10px;
  }
</style>
```
<br>

## 8.2コンポーネントの独立性とCSSの扱い
### 8.2.1 処理が含まれたコンポーネントを埋め込む
```ts
vueChapter8.2/components-vmodel/src/components/WithModel.vue

<script setup lang="ts">
  import { ref } from 'vue';
  
  const name = ref("名無し");
</script>

<template>
  <section>
    <p>{{ name }}さんですね！</p>
    <input type="text" v-model="name">
  </section>
</template>

<style scoped>
  section {
    border: orange 1px dashed;
    margin: 10px;
    padding: 10px;
  }
</style>
```
<br>

```ts
vueChapter8.2/components-vmodel/src/App.vue

<script setup lang="ts">
  import WithModel from "./components/WithModel.vue";
</script>

<template>
  <h1>コンポーネントの独立性</h1>
  <section>
    <h2>v-modelを含むコンポーネント</h2>
    <WithModel />
    <WithModel />
  </section>
</template>

<style>
  section {
    border: blue 1px solid;
    margin: 10px;
  }
</style>
```
<br>

## 8.3 親から子へのコンポーネント間通信
### 8.3.1 親からデータをもらうProps
```ts
vueChapter8.3/components-props-basics/src/components/OneInfo.vue

<script setup lang="ts">
  interface Props {
    title: string;
    content: string;
    }
  defineProps<Props>();
</script>

<template>
  <section class="box">
    <h4>{{ title }}</h4>
    <p>{{ content }}</p>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px dashed;
    margin: 10px;
  }
</style>
```
<br>

```ts
vueChapter8.3/components-props-basics/src/App.vue

<script setup lang="ts">
  import OneInfo from "./components/OneInfo.vue";
</script>

<template>
  <h1>Props基礎</h1>
  <section>
    <h2>属性に直接記述</h2>
    <OneInfo
      title="Propsの利用"
      content="子コンポーネントにデータを渡すにはPropsを利用する" />
  </section>
</template>

<style scoped>
  section {
    border: blue 1px solid;
    margin: 10px;
  }
</style>
```
<br>

### 8.3.3 親のテンプレート変数をPropsに渡す方法
```ts
vueChapter8.3/components-props-bind/src/components/OneInfo.vue

<script setup lang="ts">
  interface Props {
    title: string;
    content: number;
    }
  defineProps<Props>();
</script>

<template>
  <section class="box">
    <h4>{{ title }}</h4>
    <p>{{ content }}</p>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px dashed;
    margin: 10px;
  }
</style>
```
<br>

```ts
vueChapter8.3/components-props-bind/src/App.vue

<script setup lang="ts">
  import { ref } from "vue";
  import OneInfo from "./components/OneInfo.vue";

  const propsTitle = ref("発生した乱数");
  const rand = Math.round(Math.random() * 100);
  const propsContent = ref(rand);
</script>

<template>
  <h1>Props基礎</h1>
  <section>
    <h2>テンプレート変数を利用</h2>
    <OneInfo 
      v-bind:title="propsTitle"
      v-bind:content="propsContent"/>
  </section>
</template>
```
<br>


### 8.3.4 v-forとPropsとの組み合わせ
```ts
vueChapter8.3/components-props-vfor/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  import OneInfo from "./components/OneInfo.vue";

  const weatherListInit = new Map<number, Weather>();
  weatherListInit.set(1, {id: 1, title: "今日の天気", content: "今日は一日中、晴れでしょう"});
  weatherListInit.set(2, {id: 2, title: "明日の天気", content: "明日は一日中、雨でしょう"});
  weatherListInit.set(3, {id: 3, title: "明後日の天気", content: "明後日は一日中、雪でしょう"});
  const weatherList = ref(weatherListInit);

  interface Weather {
    id: number;
    title: string;
    content: string;
  }
</script>

<template>
  <h1>Props基礎</h1>
  <section>
    <h2>ループでコンポーネントを生成</h2>
    <OneInfo 
      v-for="[id, weather] in weatherList"
      v-bind:key="id"
      v-bind:title="weather.title"
      v-bind:content="weather.content"/>
  </section>
</template>
```
<br>

## 8.4 Propsの応用
### 8.4.1 スクリプトブロックでのPropsの値の利用
```ts
vueChapter8.4/components-props-indepth/src/components/OneMember.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';

  interface Props {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }
  const props = defineProps<Props>();
  
  const localPoints = ref(props.points);
  const localNote = computed(
    (): string => {
      let localNote = props.note;
      if(localNote == undefined) {
        localNote = "--";
        }
        return localNote;
      }
    );
  const pointUp = (): void => {
    localPoints.value++;
    }
</script>

<template>
  <section class="box">
    <h4>{{ name }}さんの情報</h4>
    <dl>
      <dt>ID</dt>
      <dd>{{ id }}</dd>
      <dt>メールアドレス</dt>
      <dd>{{ email }}</dd>
      <dt>保有ポイント</dt>
      <dd>{{ localPoints }}</dd>
      <dt>備考</dt>
      <dd>{{ localNote }}</dd>
    </dl>
    <button v-on:click="pointUp">ポイント加算</button>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
  }
</style>
```
<br>

```ts
vueChapter8.4/components-props-indepth/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';
  import OneMember from './components/OneMember.vue';

  const memberListInit = new Map<number, Member>();
  memberListInit.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com", points: 35,
  note: "初回入会特典あり"});
  memberListInit.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@example.com", points: 53});
  const memberList = ref(memberListInit);

  const totalPoints = computed(
    (): number => {
      let total = 0;
      for(const member of memberList.value.values()) {
        total += member.points;
        }
        return total;
      }
    );
  
  interface Member {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }
</script>

<template>
  <section>
    <h1>会員リスト</h1>
    <p>全会員の保有ポイントの合計：{{ totalPoints }}</p>
    <OneMember
      v-for="[id, member] in memberList"
      v-bind:key="id"
      v-bind:id="id"
      v-bind:name="member.name"
      v-bind:email="member.email"
      v-bind:points="member.points"
      v-bind:note="member.note"/>
  </section>
</template>
```
<br>

### 8.4.3 Propsのデフォルト値
```ts
vueChapter8.4/components-props-indepth2/src/components/OneMember.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';

  interface Props {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }
  const props = withDefaults(
    defineProps<Props>(),
    {note: "--"}
    );
  
  const localPoints = ref(props.points);

  const pointUp = (): void => {
    localPoints.value++;
    }
</script>

<template>
  <section class="box">
    <h4>{{ name }}さんの情報</h4>
    <dl>
      <dt>ID</dt>
      <dd>{{ id }}</dd>
      <dt>メールアドレス</dt>
      <dd>{{ email }}</dd>
      <dt>保有ポイント</dt>
      <dd>{{ localPoints }}</dd>
      <dt>備考</dt>
      <dd>{{ note }}</dd>
    </dl>
    <button v-on:click="pointUp">ポイント加算</button>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
  }
</style>
```
<br>

## 8.5 子から親へのコンポーネント間通信
### 8.5.1 子から親への通信はイベント処理
```ts
vueChapter8.5/components-emit-basics/src/components/OneSection.vue

<script setup lang="ts">
  interface Props {
    rand: number;
    }

  interface Emits {
    (event: "createNewRand"): void;
    }
  
  defineProps<Props>();
  const emit = defineEmits<Emits>();

  const onNewRandButtonClick = (): void => {
    emit("createNewRand");
    }
</script>

<template>
  <section class="box">
    <p>子コンポーネントで乱数で表示：{{ rand }}</p>
    <button v-on:click="onNewRandButtonClick">新たな乱数を発生</button>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
    }
</style>
```
<br>

```ts
vueChapter8.5/components-emit-basics/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  import OneSection from "./components/OneSection.vue";

  const randInit = Math.round(Math.random() * 10);
  const rand = ref(randInit);
  const onCreateNewRand = (): void => {
    rand.value = Math.round(Math.random() * 10);
    }
</script>

<template>
  <section>
    <p>親コンポーネントで乱数を表示：{{ rand }}</p>
    <OneSection
      v-bind:rand="rand"
      v-on:createNewRand="onCreateNewRand"/>
  </section>
</template>
```
<br>

### 8.5.2 親コンポーネントにデータを渡す方法
```ts
vueChapter8.5/components-emit-value/src/components/OneMember.vue

<script setup lang="ts">
  import { computed } from 'vue';

  interface Props {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }

    interface Emits {
      (event: "incrementPoint", id: number): void;
      }

  const props = defineProps<Props>();
  const emit = defineEmits<Emits>();
  
  const localNote = computed(
    (): string => {
      let localNote = props.note;
      if(localNote == undefined) {
        localNote = "--";
        }
        return localNote;
      }
    );
  const pointUp = (): void => {
    emit("incrementPoint", props.id);
    }
</script>

<template>
  <section class="box">
    <h4>{{ name }}さんの情報</h4>
    <dl>
      <dt>ID</dt>
      <dd>{{ id }}</dd>
      <dt>メールアドレス</dt>
      <dd>{{ email }}</dd>
      <dt>保有ポイント</dt>
      <dd>{{ points }}</dd>
      <dt>備考</dt>
      <dd>{{ localNote }}</dd>
    </dl>
    <button v-on:click="pointUp">ポイント加算</button>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
  }
</style>
```
<br>

```ts
vueChapter8.5/components-emit-value/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';
  import OneMember from './components/OneMember.vue';

  const memberListInit = new Map<number, Member>();
  memberListInit.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com", points: 35,
  note: "初回入会特典あり"});
  memberListInit.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@example.com", points: 53});
  const memberList = ref(memberListInit);

  const totalPoints = computed(
    (): number => {
      let total = 0;
      for(const member of memberList.value.values()) {
        total += member.points;
        }
        return total;
      }
    );
  const onIncrementPoint = (id: number): void => {
    const member = memberList.value.get(id);
    if(member != undefined) {
      member.points++;
      }
    }
  
  interface Member {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }
</script>

<template>
  <section>
    <h1>会員リスト</h1>
    <p>全会員の保有ポイントの合計: {{ totalPoints }}</p>
    <OneMember
      v-for="[id, member] in memberList"
      v-bind:key="id"
      v-bind:id="id"
      v-bind:name="member.name"
      v-bind:email="member.email"
      v-bind:points="member.points"
      v-bind:note="member.note"
      v-on:incrementPoint="onIncrementPoint"/>
  </section>
</template>
```
<br>

### 8.5.3 v-modelによる子から親への通信
```ts
vueChapter8.5/components-emit-model/src/components/OneMember.vue

<script setup lang="ts">
  import { computed } from 'vue';

  interface Props {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }

    interface Emits {
      (event: "update:points", points: number): void;
      }

  const props = defineProps<Props>();
  const emit = defineEmits<Emits>();
  
  const localNote = computed(
    (): string => {
      let localNote = props.note;
      if(localNote == undefined) {
        localNote = "--";
        }
        return localNote;
      }
    );

  const onInput = (event: Event): void => {
    const element = event.target as HTMLInputElement;
    const inputPoints = Number(element.value);
    emit("update:points", inputPoints);
    }
</script>

<template>
  <section class="box">
    <h4>{{ name }}さんの情報</h4>
    <dl>
      <dt>ID</dt>
      <dd>{{ id }}</dd>
      <dt>メールアドレス</dt>
      <dd>{{ email }}</dd>
      <dt>保有ポイント</dt>
      <dd>
        <input type="number" v-bind:value="points" v-on:input="onInput">
      </dd>
      <dt>備考</dt>
      <dd>{{ localNote }}</dd>
    </dl>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
  }
</style>
```
<br>

```ts
vueChapter8.5/components-emit-model/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';
  import OneMember from './components/OneMember.vue';

  const memberListInit = new Map<number, Member>();
  memberListInit.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com", points: 35,
  note: "初回入会特典あり"});
  memberListInit.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@example.com", points: 53});
  const memberList = ref(memberListInit);

  const totalPoints = computed(
    (): number => {
      let total = 0;
      for(const member of memberList.value.values()) {
        total += member.points;
        }
        return total;
      }
    );
  
  interface Member {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }
</script>

<template>
  <section>
    <h1>会員リスト</h1>
    <p>全会員の保有ポイントの合計：{{ totalPoints }}</p>
    <OneMember
      v-for="[id, member] in memberList"
      v-bind:key="id"
      v-bind:id="id"
      v-bind:name="member.name"
      v-bind:email="member.email"
      v-model:points="member.points"
      v-bind:note="member.note"/>
  </section>
</template>
```
<br>

## 8.6 ProvideとInject
### 8.6.2 サンプルプロジェクトの作成
```ts
vueChapter8.6/components-provinje/src/interfaces.ts

export interface Member {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
}
```
<bue>

```ts
vueChapter8.6/components-provinje/src/components/OneMember.vue

<script setup lang="ts">
  import { computed, inject } from 'vue';
  import type { Member } from '../interfaces';

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
      let localNote = member.value.note;
      if(localNote == undefined) {
        localNote = "--";
        }
        return localNote;
      }
    );
</script>

<template>
  <section class="box">
    <h4>{{ member.name }}さんの情報</h4>
    <dl>
      <dt>ID</dt>
      <dd>{{ id }}</dd>
      <dt>メールアドレス</dt>
      <dd>{{ member.email }}</dd>
      <dt>保有ポイント</dt>
      <dd>
        <input type="number" v-model.number="member.points">
      </dd>
      <dt>備考</dt>
      <dd>{{ localNote }}</dd>
    </dl>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
  }
</style>
```
<bue>

```ts
vueChapter8.6/components-provinje/src/components/BaseSection.vue

<script setup lang="ts">
    import { computed, inject } from 'vue';
    import OneMember from './OneMember.vue';
    import type { Member } from '../interfaces';

    const memberList = inject("memberList") as Map<number, Member>;
    const totalPoints = computed(
        (): number => {
            let total = 0;
            for(const member of memberList.values()) {
                total += member.points;
                }
                return total;
            }
        );
</script>

<template>
    <section>
        <h1>会員リスト</h1>
        <p>全会員の保有ポイントの合計：{{ totalPoints }}</p>
        <OneMember
            v-for="id in memberList.keys()"
            v-bind:key="id"
            v-bind:id="id"/>
    </section>
</template>

<style scoped>
    section {
        border: orange 1px dashed;
        margin: 10px;
    }
</style>
```
<bue>

```ts
vueChapter8.6/components-provinje/src/App.vue

<script setup lang="ts">
  import { reactive, provide } from 'vue';
  import BaseSection from './components/BaseSection.vue';
  import type { Member } from './interfaces';

  const memberList = new Map<number, Member>();
  memberList.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com", points: 35,
  note: "初回入会特典あり"});
  memberList.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@example.com", points: 53});
  provide("memberList", reactive(memberList));
</script>

<template>
  <BaseSection />
</template>
```