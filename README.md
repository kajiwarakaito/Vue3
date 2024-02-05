## 「Vue3 フロントエンド開発の教科書」5章  
## 5.1 双方向データバインディング
### 5.1.1双方向データバインディングを実現するv-model
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
デフォルトではテキストエリアのinput、その下に「双方向」というテキストが表示されており、テキストエリアに入力した内容を反映し、テキストとして下に表示するプログラム  
<br>

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
テキストエリアのinputが二つ、上のinputにデフォルトで「しんちゃん」と表示されており、下のinputには何もない。inputの下には「ななし」というテキストがあり、下のinputに文字を入力することで反映され、データの受け渡しがされる
<br>

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