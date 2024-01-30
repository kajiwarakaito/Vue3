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
**setup**  
この文言によっていろいろと設定の手順を省いて記述できるようになる。  
逆にsetupに入っている設定をデフォルトから変えたい場合は、  
setupを抜いて記述しなければならない→　7章参照  

**{{name}}**  
マスタッシュ構文。スクリプトブロックで宣言し、テンプレート変数にした物を当てはめて表示する  

**import {ref} from "vue;"**  
これによってsetupで用意したデフォルト設定から引き込んできて{}内の機能を使えるようにする  

**const**  
宣言。これによって変数等を定義する。  

**ref()**  
ref関数の()内に引数を渡すことで、その戻り値をマスタッシュ構文の変数に当てはめることが出来る  

<br>
つまり

```ts
const name = ref("田中太郎");
```
によって変数:nameを宣言、同時にref関数の引数に"田中太郎"を入れる事によって、
変数:nameにref関数の戻り値を代入する。(要するにnameに"田中太郎"を代入)  
