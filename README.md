# Vue3.Chapter3
## 「Vue3 フロントエンド開発の教科書」3章  
### 3.1.2 単一コンポーネントファイルとは

主にscript, template, styleの三つから構成されている。  
この構成からなるファイルを単一コンポーネントファイルといい、  
拡張子は" .vue "となる。  
①：スクリプトブロック。JavaScript(TypeScript)コードを枠内に記述。  
②：テンプレートブロック。HTMLタグを記述。  
③：スタイルブロック。CSSコードを記述。  
必要に応じて削除するとわかりやすい  

```ts
App.vue
<script setup lang="ts">
｜
</script>

<template>
｜
</template>

<style>
｜
</style>
```

### 3.1.3 記述の基本

```ts
mustache/src/App.vue
<script setup lang="ts">
    import {ref} from "vue";

    const name = ref("田中太郎");
</script>

<template>
    <h1>こんにちは！{{name}}さん！</h1>
</template>
```