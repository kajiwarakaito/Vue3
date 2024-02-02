## 「Vue3 フロントエンド開発の教科書」5章  
## 5.1 双方向データバインディング
```ts
vueChapter5.1/directive-model/directive-model/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const inputNameModel = ref("双方向");
</script>

<template>
  <section>
    <input type="text" v-model="inputNameModel">
    <p>{{ inputNameModel }}</p>
  </section>
</template>
```

```ts
vueChapter5.1/directive-oneway/directive-oneway/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const inputNameBind = ref("しんちゃん");
  const inputNameOn = ref("ななし");
  const onInputName = (event: Event): void => {
    const element = event.target as HTMLInputElement;
    inputNameOn.value = element.value;
    }
</script>

<template>
  <section>
    <input type="text" v-bind:value="inputNameBind">
  </section>
  <br>
  <section>
    <input type="text" v-on:input="onInputName">
    <p>{{ inputNameOn }}</p>
  </section>
</template>
```

```ts
```

```ts
```

```ts
```

```ts
```

```ts
```

```ts
```

## 5.2 その他のデータバインディングのディレクティブ
```ts
```