## 「Vue3 フロントエンド開発の教科書」6章  
## 6.1 条件分岐のディレクティブ
### 6.1.1 条件分岐ディレクティブの基本v-if
```ts
vueChapter6.1/directive-conditional-basic/directive-conditional-basic/src/App.vue

<script setup lang="ts">
  import { computed, ref } from 'vue';
  
  const number = ref(80);
  const showOrNot = computed(
    (): boolean => {
      let showOrNot = false;
      const rand = Math.round(Math.random() * 100);
      if(rand >= 50) {
        showOrNot = true;
        }
      return showOrNot;
      }
    );
</script>

<template>
  <p v-if="number >= 50">
    条件に合致したので表示1
  </p>
  <p v-if="Math.round(Math.random() * 100) >= 50">
    条件に合致したので表示2
  </p>
  <p v-if="showOrNot">
    条件に合致したので表示3
  </p>
</template>
```
<br>

### 6.1.2 条件分岐ディレクティブをフルセットで利用した場合
```ts
vueChapter6.1/directive-conditional-full/directive-conditional-full/src/App.vue

<script setup lang="ts">
  import { computed } from 'vue';

  const randomNumber = computed(
    (): number => {
      return Math.round(Math.random() * 100);
      }
    );
</script>

<template>
  <p>
    点数は{{ randomNumber }}点で
    <span v-if="randomNumber >= 80">優です。</span>
    <span v-else-if="randomNumber >= 70">良です。</span>
    <span v-else-if="randomNumber >= 60">可です。</span>
    <span v-else>不可です。</span>
  </p>
</template>
```
<br>

### 6.1.3 ディレクティブ記述の為のtemplate
```ts
vueChapter6.1/directive-conditional-template/directive-conditional-template/src/App.vue

<script setup lang="ts">
  import { computed } from 'vue';

  const randomNumber = computed(
    (): number => {
      return Math.round(Math.random() * 100);
      }
    );
</script>

<template>
  <p>
    点数は{{ randomNumber }}点
    <template v-if="randomNumber >= 80">
      で優です。
      <span style="color: red;">すばらしい！</span>
    </template>
  </p>
</template>
```
<br>

### 6.1.4 v-ifと似て非なるv-show
```ts
vueChapter6.1/directive-show/directive-show/src/App.vue

<script setup lang="ts">
import { computed } from 'vue';

  const showOrNot = computed(
    (): boolean => {
      //戻り値用変数を初期値falseで用意
      let showOrNot = false;
      //0~100の乱数を発生
      const rand = Math.round(Math.random() * 100);
      //乱数が50以上ならば戻り値をtrueに変更
      if(rand >= 50) {
        showOrNot = true;
        }
      return showOrNot;
      }
  );
</script>

<template>
  <section>
    v-ifを利用
    <p v-if="showOrNot">条件に合致したので表示</p>
  </section>
  <section>
    v-showを利用
    <p v-show="showOrNot">条件に合致したので表示</p>
  </section>
</template>
```
<br>

## 6.2 ループのディレクティブ
### 6.2.1 ループのディレクティブであるv-for
```ts
vueChapter6.2/a/a/src/App.vue

```
<br>

### 6.2.1 ループのディレクティブであるv-for
```ts
vueChapter6.2/a/a/src/App.vue

```
<br>

### 6.2.1 ループのディレクティブであるv-for
```ts
vueChapter6.2/a/a/src/App.vue

```
<br>

### 6.2.1 ループのディレクティブであるv-for
```ts
vueChapter6.2/a/a/src/App.vue

```
<br>

### 6.2.1 ループのディレクティブであるv-for
```ts
vueChapter6.2/a/a/src/App.vue

```
<br>

### 6.2.1 ループのディレクティブであるv-for
```ts
vueChapter6.2/a/a/src/App.vue

```
<br>

### 6.2.1 ループのディレクティブであるv-for
```ts
vueChapter6.2/a/a/src/App.vue

```
<br>

## 6.3 リスト操作
### 6.3.1 ループ対象データの絞り込み
```ts
vueChapter6.3/a/a/src/App.vue

```
<br>

### 6.3.1 ループ対象データの絞り込み
```ts
vueChapter6.3/a/a/src/App.vue

```
<br>

### 6.3.1 ループ対象データの絞り込み
```ts
vueChapter6.3/a/a/src/App.vue

```
<br>

### 6.3.1 ループ対象データの絞り込み
```ts
vueChapter6.3/a/a/src/App.vue

```
<br>

### 6.3.1 ループ対象データの絞り込み
```ts
vueChapter6.3/a/a/src/App.vue

```
<br>