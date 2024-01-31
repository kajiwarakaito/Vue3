## 「Vue3 フロントエンド開発の教科書」4章  
## 4.1 データバインディングのディレクティブ
```ts
<script setup lang="ts">
  import {ref} from "vue";

  const url = ref("https://vuejs.org/");
</script>

<template>
  <p><a v-bind:href="url" target="_blank">Vue.jsのサイト</a></p>
  <p><a :href="url" target="_blank">Vue.jsのサイト(省略形)</a></p>
  <p>
    <a v-bind:href="url + 'guide/introduction.html'" target="_blank">
      Vue.jsガイドのページ
    </a>
  </p>
</template>
```
3つのリンクが生成される  
レンダリングされたコードを確認する場合は、ブラウザにF12を行い、デベロッパーツールにてElementsを確認、➤で閉じられているためクリックして確認する。  
[後で画像差し込み]  
<br>

v-bindの基本構文
```ts
v-bind:href="url"
```
省略系↓
```ts
:href="url"
```
式もいける
```ts
"url + 'guide/introduction.html'"
```
文は無理
```ts
"let url = 'guide/introduction.html'"
```


## 4.2 イベントのディレクティブ
