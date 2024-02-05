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

### 5.1.2片方向のデータバインディング
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

### 5.1.3 v-bindとv-on:inputの組み合わせ
```ts
vueChapter5.1/directive-twoway/directive-twoway/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  
  const inputName2way = ref("双方向");
  const onInputName2Way = (event: Event): void => {
    const element = event.target as HTMLInputElement;
    inputName2way.value = element.value;
    }
</script>

<template>
  <section>
    <input type="text" v-bind:value="inputName2way" v-on:input="onInputName2Way">
    <p>{{ inputName2way }}</p>
  </section>
</template>
```
v-bindとv-on:inputを駆使し、テンプレート変数を同じ物にすることで双方向データバインディングすることが出来るようにしている  
→ 5.1.1のv-modelはこれをシンプルにしたもの  
テンプレート変数もマスタッシュ構文もリアクティブ化しているのでリアルタイムで変更できる  
<br>

### 5.1.4 文字列入力以外のコントロールでのv-model
```ts
vueChapter5.1/directive-model-controls/directive-model-controls/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const inputTextarea = ref("テキストエリアへの入力文字。\n改行も加えてみる。");
  const memberType = ref(1);
  const memberTypeSelect = ref(1);
  const isAgreed = ref(false);
  const isAgreed01 = ref(0);
  const selectedOS = ref([]);
  const selectedOSSelect = ref([]);
</script>

<template>
  <textarea v-model="inputTextarea"></textarea>
  <br>
  <section>
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
  </section>

  <br>
  <section>
    <select v-model="memberTypeSelect">
      <option value="1">通常会員</option>
      <option value="2">特別会員</option>
      <option value="3">優良会員</option>
    </select>
    <br>
    <p>選択されたリスト：{{ memberTypeSelect }}</p>
  </section>


  <br>
  <section>
    <label>
      <input type="checkbox" v-model="isAgreed">
      同意する
    </label>
    <p>同意の結果：{{ isAgreed }}</p>
  </section>
  <section>
    <label>
      <input type="checkbox" v-model="isAgreed01" true-value="1" false-value="0">
      同意する
    </label>
    <p>同意の結果：{{ isAgreed01 }}</p>
  </section>
  <section>
    <label>
      <input type="checkbox" v-model="selectedOS" value="1">
      macOS
    </label>
    <label>
      <input type="checkbox" v-model="selectedOS" value="2">
      Windows
    </label>
    <label>
      <input type="checkbox" v-model="selectedOS" value="3">
      Linux
    </label>
    <label>
      <input type="checkbox" v-model="selectedOS" value="4">
      iOS
    </label>
    <label>
      <input type="checkbox" v-model="selectedOS" value="5">
      Android
    </label>
    <p>選択されたOS: {{ selectedOS }}</p>
  </section>
  <section>
    <select v-model="selectedOSSelect" multiple>
      <option value="1">macOS</option>
      <option value="2">Windows</option>
      <option value="3">Linux</option>
      <option value="4">iOS</option>
      <option value="5">Android</option>
    </select>
    <p>選択されたOS: {{ selectedOSSelect }}</p>
  </section>
</template>
```
いろいろなinputを使った双方向データバインディングをまとめたもの
1. テキストエリア
2. 横並びの選択ボタン。選択に応じて下のテキストのラジオボタンの番号がかわる
3. ラベルリストを選択、その選択に応じて下のテキストの番号が変わる
4. チェックボックス形式。是非によりtrue, falseいずれかになる
5. チェックボックス形式。是非によりカウントが1増えたり0に戻ったりする
6. チェックボックス形式。チェックしたボックスに応じて下のテキストの表示が変わる
7. スクロールバー形式。選択したリストに応じて下のテキストの表示が変わる
<br>

### 5.1.5 v-modelの修飾子
```ts
vueChapter5.1/directive-model-trim/directive-model-trim/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const trimedText = ref("");
</script>

<template>
  <section>
    <input type="text" v-model.trim="trimedText">
    <p>入力文字列：{{ trimedText }}</p>
  </section>
</template>
```
v-model".trim"により空白をいれた文字列でもそれを取り除いたうえで表示する。  
<br>

## 5.2 その他のデータバインディングのディレクティブ
### 5.2.1 HTML文字列をそのまま表示するv-html
```ts
vueChapter5.2/directive-html/directive-html/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  const htmlStr = ref(`<a href="https://vuejs.org//">Vue.jsのTOPページ<a>`);
</script>

<template>
  <section>{{ htmlStr }}</section>
  <section v-html="htmlStr"></section>
</template>
```
リアクティブになった変数としてそのままマスタッシュ構文にした上はhtml文がそのまま表示されるが、v-htmlを付けた下はhtmlの様にハイパーリンクになって表示される
<br>

### 5.2.2 静的コンテンツとして表示させるv-pre
```ts
vueChapter5.2/directive-pre/directive-pre/src/App.vue

<script setup lang="ts">
</script>

<template>
  <section v-pre>
    <p v-on:click="showHello">{{ hello! }}</p>
  </section>
</template>
```
これで囲まれた中身をマスタッシュ構文も含めて全て無効化、そのままテキスト表示するディレクティブ
<br>

### 5.2.3 データバインドを一度だけ行うv-once
```ts
vueChapter5.2/directive-once/directive-once/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const price = ref(1000);
</script>

<template>
  <section>
    <input type="number" v-model="price">
    円<br>
    <p>金額は{{ price }}円です。</p>
    <p v-once>金額は{{ price }}円です。</p>
  </section>
</template>
```
テキストエリアのinputと下にテキスト二つがならんでおり、デフォルトで1000が入力されている。v-onceはリアクティブな値であっても最初の一回だけの入力を受け付けるため、input内の数値を変更しても上のマスタッシュ構文を使用したテキストは変更されるが下のv-onceを使用したマスタッシュ構文では初期値から変更されない  
<br>

### 5.2.4 レンダリング終了までマスタッシュ構文を非表示にするv-cloak
```ts
vueChapter5.2/directive-cloak/directive-cloak/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  const hello = ref("こんにちは！");
</script>

<template>
  <p v-cloak>{{ hello }}</p>
</template>

<style>
  [v-cloak] {
    display: none;
    }
</style>
```
ブラウザの動作によりマスタッシュ構文が表示されてしまうことがある。なのでレンダリングして表示するまでマスタッシュ構文を晒さないために用いる