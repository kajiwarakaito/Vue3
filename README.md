## 「Vue3 フロントエンド開発の教科書」9章
## 9.1 子コンポーネントをカスタマイズするSlot
### 9.1.2 Slotの基本的な記述法
```ts
vueChapter9.1/slot-basic/src/components/OneSection.vue

<script setup lang="ts">
  interface Props {
    name: string;
  }
  defineProps<Props>();
</script>

<template>
  <section class="box">
    <h1>{{ name }}さんの状況</h1>
    <slot />
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
vueChapter9.1/slot-basic/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  import OneSection from './components/OneSection.vue';

  const taro = ref("田中太郎");
</script>

<template>
  <section>
    <h2>Slotの利用</h2>
    <OneSection v-bind:name="taro">
      <p>連絡がつきません</p>
    </OneSection>
  </section>
</template>
```
<br>

### 9.1.3 Slotのフォールバックコンテンツ
```ts
vueChapter9.1/slot-fallback/src/components/OneSection.vue

<script setup lang="ts">
  interface Props {
    name: string;
  }
  defineProps<Props>();
</script>

<template>
  <section class="box">
    <h1>{{ name }}さんの状況</h1>
    <slot>
      <p>問題ありません</p>
    </slot>
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
vueChapter9.1/slot-fallback/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  import OnSection from "./components/OneSection.vue";

  const taro = ref("田中太郎");
  const jiro = ref("鈴木二郎");
</script>

<template>
  <section>
    <h2>Slotの利用</h2>
    <OnSection v-bind:name="taro">
      <p>連絡がつきません</p>
    </OnSection>
    <OnSection v-bind:name="jiro"/>
  </section>
</template>
```
<br>

### 9.1.4
```ts
vueChapter9.1/slot-datascope/src/components/OneSection.vue

<script setup lang="ts">
  interface Props {
    name: string;
  }
  defineProps<Props>();
</script>

<template>
  <section class="box">
    <h1>{{ name }}さんの状況</h1>
    <slot>
      <p>{{name}}さんは問題ありません</p>
    </slot>
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
vueChapter9.1/slot-datascope/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  import OnSection from "./components/OneSection.vue";

  const taro = ref("田中太郎");
  const jiro = ref("鈴木二郎");
</script>

<template>
  <section>
    <h2>Slotの利用</h2>
    <OnSection v-bind:name="taro">
      <p>{{ taro }}さんは連絡がつきません</p>
    </OnSection>
    <OnSection v-bind:name="jiro"/>
  </section>
</template>
```
<br>

### 9.1.5
```ts
vueChapter9.2/slot-dynamic/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  import OneSection from "./components/OneSection.vue";

  const taroProblemsInit: string[] = ["電話が通じません", "留守です"];
  const taro = ref("田中太郎");
  const taroProblems = ref(taroProblemsInit);
  const jiro = ref("鈴木二郎");
</script>

<template>
  <section>
    <OneSection v-bind:name="taro">
      <ul>
        <li v-for="problem in taroProblems" v-bind:key="problem">
          {{ problem }}
        </li>
      </ul>
    </OneSection>
    <OneSection v-bind:name="jiro" />
  </section>
</template>
```
<br>

## 9.2 複数のSlotを実現する名前付きSlot
### 9.2.2 名前付きSlot
```ts
vueChapter9.2/slot-named/src/components/OneSection.vue

<script setup lang="ts">
  interface Props {
    name: string;
  }
  defineProps<Props>();
</script>

<template>
  <section class="box">
    <h1>{{ name }}さんの状況</h1>
    <slot>
      <p>問題ありません</p>
    </slot>
    <h4>詳細内容</h4>
    <slot name="detail">
      <p>特にありません</p>
    </slot>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
  }
  h1 {
    font-weight: bold;
  }
</style>
```
<br>

```ts
vueChapter9.2/slot-named/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  import OneSection from "./components/OneSection.vue";

  const taroProblemsInit: string[] = ["電話が通じません", "留守です"];
  const taro = ref("田中太郎");
  const taroProblems = ref(taroProblemsInit);
  const jiro = ref("鈴木二郎");
</script>

<template>
  <section>
    <OneSection v-bind:name="taro">
      <template v-slot:default>
        <p>問題発生</p>
      </template>
    <template v-slot:detail>
      <ul>
        <li v-for="problem in taroProblems" v-bind:key="problem">
          {{ problem }}
        </li>
      </ul>
    </template>
    </OneSection>
    <OneSection v-bind:name="jiro" />
  </section>
</template>
```
<br>

## 9.3 データの受け渡しを逆転させるスコープ付きSlot
### 9.3.1 スコープ付きSlotとは
```ts
vueChapter9.3/slot-scoped/src/components/OneSection.vue

<script setup lang="ts">
  import { reactive } from 'vue';

  const memberInfo = reactive({
    name: "田中太郎",
    state: "問題ありません"
  });
</script>

<template>
  <section>
    <slot v-bind:memberInfo="memberInfo">
      <h1>{{ memberInfo.name }}さんの状況</h1>
      <p>{{ memberInfo.state }}</p>
    </slot>
  </section>
</template>
```
<br>

```ts
vueChapter9.3/slot-scoped/src/App.vue

<script setup lang="ts">
  import OneSection from "./components/OneSection.vue";
</script>

<template>
  <section>
    <OneSection>
      <template v-slot:default="slotProps">
      <dl>
        <dt>名前</dt>
        <dd>{{ slotProps.memberInfo.name }}</dd>
        <dt>状況</dt>
        <dd>{{ slotProps.memberInfo.state }}</dd>
      </dl>
    </template>
    </OneSection>
  </section>
</template>
```
<br>

## 9.4 動的コンポーネント
### 9.4.1 動的コンポーネントとは
```ts
vueChapter9.4/components-dynamic/src/components/Input.vue

<script setup lang="ts">
  import { ref } from 'vue';
  const inputNameModel = ref("田中太郎");
</script>

<template>
  <input type="text" v-model="inputNameModel">
  <p>{{ inputNameModel }}</p>
</template>
```
<br>

```ts
vueChapter9.4/components-dynamic/src/components/Radio.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const memberType = ref(1);
</script>

<template>
  <label>
    <input type="radio" name="memberType" value="1" v-model="memberType">
    通常会員
  </label>
  <label>
    <input type="radio" name="memberType" value="2" v-model="memberType">
    特別会員
  </label>
  <label>
    <input type="radio" name="memberType" value="3" v-model="memberType">
    優良会員
  </label>
  <br>
  <p>選択されたラジオボタン：{{ memberType }}</p>
</template>
```
<br>

```ts
vueChapter9.4/components-dynamic/src/components/Select.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const memberTypeSelect = ref(1);
</script>

<template>
  <select v-model="memberTypeSelect">
    <option value="1">通常会員</option>
    <option value="2">特別会員</option>
    <option value="3">優良会員</option>
  </select>
  <br>
  <p>選択されたリスト：{{ memberTypeSelect }}</p>
</template>
```
<br>

```ts
vueChapter9.4/components-dynamic/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  import Input from "./components/Input.vue";
  import Radio from "./components/Radio.vue";
  import Select from "./components/Select.vue";

  const currentComp = ref(Input);
  const currentCompName = ref("Input");
  const compList = [Input, Radio, Select];
  const compNameList: string[] = ["Input", "Radio", "Select"];
  let currentCompIndex = 0;
  const switchComp = (): void => {
    currentCompIndex++;
    if(currentCompIndex >= 3) {
      currentCompIndex = 0;
    }
    currentComp.value = compList[currentCompIndex];
    currentCompName.value = compNameList[currentCompIndex];
  }
</script>

<template>
  <p>コンポーネント名：{{ currentCompName }}</p>
  <KeepAlive>
    <component v-bind:is="currentComp"/>
  </KeepAlive>
  <button v-on:click="switchComp">切り替え</button>
</template>
```
<br>
