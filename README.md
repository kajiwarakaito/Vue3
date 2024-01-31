## 「Vue3 フロントエンド開発の教科書」3章  
## 3.1 Vueのコンポーネントと基本構文

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

<br>

Hava bad day! ...Hey, Kajy!
```ts
mustache/src/App.vue

<script setup lang="ts">
    import {ref} from "vue";

    const name = ref("梶原海斗");
</script>

<template>
    <h1>こんにちは！{{name}}さん！</h1>
</template>
```
<br>
```ts
setup
```
この文言によっていろいろと設定の手順を省いて記述できるようになる。  
逆にsetupに入っている設定をデフォルトから変えたい場合は、  
setupを抜いて記述しなければならない→　7章参照  
<br>

```ts
{{name}}
```  
マスタッシュ構文。スクリプトブロックで宣言し、テンプレート変数にした物を当てはめて表示する  
<br>

```ts
import {ref} from "vue";
```  
これによってsetupで用意したデフォルト設定から引き込んできて{}内の機能を使えるようにする  
<br>

```ts
const
```  
宣言。これによって変数等を定義する。  
<br>

```ts
ref()
```  
ref関数の()内に引数を渡すことで、その戻り値をマスタッシュ構文の変数に当てはめることが出来る  
<br>

つまり
```ts
const name = ref("梶原海斗");
```
によって変数:nameを宣言、同時にref関数の引数に"田中太郎"を入れる事によって、
変数:nameにref関数の戻り値を代入する。(要するにnameに"梶原海斗"を代入)  
<br>

```ts
const Name = "梶原海斗";
const name = ref(Name);
```
一気にやらず一つ一つやることも当然できる

## 3.2 リアクティブシステム
```ts
reactive-system/src/App.vue

<script setup lang="ts">
    import {ref} from "vue";

    const now = new Date();
    const nowStr = now.toLocaleTimeString();
    let timeStr = nowStr;
    const timeStrRef = ref(nowStr);
    ↑ 現在時刻とその文字列をテンプレート変数として用意

    新しい時刻に変更・更新するchangeTime関数
    function changeTime(): void{
        const newTime = new Date();
        const newTimeStr = newTime.toLocaleTimeString();
        ↑ 現在の時刻とその文字列を取得

        timeStr = newTimeStr;
        timeStrRef.value = newTimeStr;
        ↑ 現在の時刻文字列をそれぞれのテンプレート変数に代入
        }

    changeTime関数を1秒ごとに更新させる
    setInterval(changeTime, 1000);
</script>

<template>
    <p>現在時刻: {{ timeStr }}</p>
    <p>現在時刻(ref): {{ timeStrRef }}</p>
</template>
```
上記のコードの内容の要約としては、一文の前後に現在時刻を表示し、前に更新した瞬間の時刻を表示、後ろに更新し続ける現在時刻を表示する  
<br>

```ts
const timeStrRef = ref(nowStr);
```
この一文により、変数timeStrRefに代入する値をnowStrのリアクティブ変数と定める  
こうすることにより 
```ts
timeStrRef.value = newTimeStr;
```
によって時刻が変化する  
<br>

リアクティブな状態を維持しないと更新されないので、変数timeStrRefに **.value** をつけ、timeStrRef.valueとすることで現在時刻を更新し続けることができる

## 3.3 リアクティブデータ用意のバリエーション
スクリプトブロック、外部からの入力によって与えられた変数をリアクティブデータとして扱いたい時等では算出プロパティという方法を使うのがいい
```ts
computed/src/App.vue

<script setup lang="ts">
  import {ref, computed, reactive} from "vue";

  const radiusInit = Math.round(Math.random() * 10);
  const PI = ref(3.14);
  const radius = ref(radiusInit);

  ↓ 算出プロパティを computed()関数を使ってテンプレート変数に
  const area = computed(
    (): number => {
      return radius.value * radius.value * PI.value;
      }
    );

  setInterval(
    (): void => {
      radius.value = Math.round(Math.random() * 10);
      }, 1000
  );
</script>

<template>
  <p>半径{{ radius }}の円の面積を円周率{{ PI }}で計算すると、{{ area }}</p>
</template>
```
算出プロパティ(Computed Propperties)を使うことにより、スクリプトブロックや外部からの入力によって与えられた変数をリアクティブデータとして取り扱うことが出来る。  
ここでは計算式内の数値を1秒ごとにランダムに変化させ、計算を行うため使用している  



reactive()関数：複数のデータを一つにまとめ上げ、まとめてリアクティブ化する変数

```ts
<script setup lang="ts">
  import { reactive, computed } from 'vue';

  const data = reactive({
    PI: 3.14,
    radius: Math.round(Math.random() * 10)
  });
  const area = computed(
    (): number => {
      return data.radius * data.radius * data.PI;
    }
  );
  setInterval(
    ():void => {
      data.radius = Math.round(Math.random() * 10);
    },
    1000
  );
</script>

<template>
  <p>半径{{ data.radius }}の円の面積を円周率{{ data.PI }}で計算すると、{{ area }}</p>
</template>
```
上記のコードのここ
```ts
  const data = reactive({
    PI: 3.14,
    radius: Math.round(Math.random() * 10)
  });
```
でreactive関数を用いてまとめて宣言しているのがわかる

## 3.4 Vueプロジェクトの構成と動作原理

`npm run build`  
作成したいVueのプロジェクトフォルダ上でこのコマンドをコンソールで走らせることにより、htmlやcssファイル等Webサイトを立ち上げるにつき必要なものを纏めて生成することができる  
