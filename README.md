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
vueChapter6.2/directive-loop-basic/directive-loop-basic/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const cocktailListInit: string[] = ["ホワイトレディ", "ブルーハワイ", "ニューヨーク"];
  const cocktailList = ref(cocktailListInit);
</script>

<template>
  <ul>
    <li v-for="cocktailName in cocktailList"
      v-bind:key="cocktailName">
      {{ cocktailName }}
    </li>
  </ul>
  <ul>
    <li v-for="(cocktailName, index) in cocktailList"
      v-bind:key="cocktailName">
      {{ cocktailName }}(インデックス{{ index }})
    </li>
  </ul>
</template>
```
<br>

### 6.2.2 連想配列のループ
```ts
vueChapter6.2/directive-loop-associative/directive-loop-associative/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const cocktailListInit: {[key: number]: string;} = {
    2345: "ホワイトレディ",
    4412: "ブルーハワイ",
    6792: "ニューヨーク"
    };
  const cocktailList = ref(cocktailListInit);
</script>

<template>
  <ul>
    <li
      v-for="(cocktailName, id) in cocktailList"
      v-bind:key="'cocktailList' + id">
      IDが{{ id }}のカクテルは{{ cocktailName }}
    </li>
  </ul>
  <ul>
    <li
      v-for="(cocktailName, id, index) in cocktailList"
      v-bind:key="'cocktailListWithIdx' + id">
      {{ index + 1 }}: IDが{{ id }}のカクテルは{{ cocktailName }}
    </li>
  </ul>
</template>
```
<br>

### 6.2.3 Mapのループ
```ts
vueChapter6.2/directive-loop-map/directive-loop-map/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const cocktailListInit = new Map<number, string>();
  cocktailListInit.set(2345, "ホワイトレディ");
  cocktailListInit.set(4412, "ブルーハワイ");
  cocktailListInit.set(6792, "ニューヨーク");
  const cocktailList = ref(cocktailListInit);
</script>

<template>
  <ul>
    <li
      v-for="[id, cocktailName] in cocktailList"
      v-bind:key="id">
      IDが{{ id }}のカクテルは{{ cocktailName }}
    </li>
  </ul>
</template>
```
<br>

### 6.2.4 オブジェクトのループ
```ts
vueChapter6.2/directive-loop-obj/directive-loop-obj/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';
  const whiteLadyInit: {
    id: number;
    name: string;
    price: number;
    recipe: string;
    } = {
      id: 2345,
      name: "ホワイトレディ",
      price: 1200,
      recipe: "ジン30ml+コワントロー15ml+レモン果汁15ml"
      };
  const whiteLady = ref(whiteLadyInit);
</script>

<template>
  <dl>
    <template
      v-for="(value, key) in whiteLady"
      v-bind:key="key">
      <dt>{{ key }}</dt>
      <dd>{{ value }}</dd>
    </template>
  </dl>
</template>
```
<br>

### 6.2.5 オブジェクトの配列のループ
```ts
vueChapter6.2/directive-loop-arrayobj/directive-loop-arrayobj/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const cocktailDataListInit: Cocktail[] = [
    {id: 2345, name: "ホワイトレディ", price: 1200},
    {id: 4412, name: "ブルーハワイ", price: 1500},
    {id: 6792, name: "ニューヨーク", price: 1100},
    {id: 8429, name: "マティーニ", price: 1500}
    ];
  const cocktailDataList = ref(cocktailDataListInit);

  interface Cocktail {
    id: number;
    name: string;
    price: number;
    }
</script>

<template>
  <ul>
    <li
      v-for="cocktailItem in cocktailDataList"
      v-bind:key="cocktailItem.id">
      {{ cocktailItem.name }}の値段は{{ cocktailItem.price }}円です
    </li>
  </ul>
</template>
```
<br>

### 6.2.6 Mapとオブジェクトの組み合わせのループ
```ts
vueChapter6.2/directive-loop-mapobj/directive-loop-mapobj/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const cocktailDataListInit = new Map<number, Cocktail>();
  cocktailDataListInit.set(2345, {id: 2345, name: "ホワイトレディ", price: 1200});
  cocktailDataListInit.set(4412, {id: 4412, name: "ブルーハワイ", price: 1500});
  cocktailDataListInit.set(6792, {id: 6792, name: "ニューヨーク", price: 1100});
  cocktailDataListInit.set(8429, {id: 8429, name: "マティーニ", price: 1500});
  const cocktailDataList = ref(cocktailDataListInit);

  interface Cocktail {
    id: number;
    name: string;
    price: number;
  }
</script>

<template>
  <ul>
    <li
      v-for="[id, cocktailItem] in cocktailDataList"
      v-bind:key="id">
      {{ cocktailItem.name }}の値段は{{ cocktailItem.price }}円です
    </li>
  </ul>
</template>
```
<br>

### 6.2.7 カウンタ変数を利用したループ
```ts
vueChapter6.2/directive-loop-range/directive-loop-range/src/App.vue

<template>
  <ul>
    <li
      v-for="r in 5"
      v-bind:key="r">
      半径{{ r }}の円の円周: {{ 2 * r * 3.14 }}
    </li>
  </ul>
</template>
```
<br>

## 6.3 リスト操作
### 6.3.1 ループ対象データの絞り込み
```ts
vueChapter6.3/listchange-filter/listchange-filter/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';

  const cockteilDataListInit: Cocktail[] = [
    {id: 2345, name: "ホワイトレディ", price: 1200},
    {id: 4412, name: "ブルーハワイ", price: 1500},
    {id: 6792, name: "ニューヨーク", price: 1100},
    {id: 8429, name: "マティーニ", price: 1500}
    ];
  const cocktailDataList = ref(cockteilDataListInit);
  const cocktail1500 = computed(
    (): Cocktail[] => {
      //配列のfilter()メソッドを使って新たな配列を生成
      const newList = cocktailDataList.value.filter(
        //filter()メソッドの絞り込み条件関数
        //因数は配列の各要素であるCocktailオブジェクト
        (cocktailItem: Cocktail): boolean => {
          //値段が1500かどうかの結果を戻り値とする
          return cocktailItem.price ==1500;
          }
        );
      return newList;
      }
    );
  
  interface Cocktail {
    id: number;
    name: string;
    price: number;
    }
</script>

<template>
  <section>
    全てのカクテルリスト
    <ul>
      <li
        v-for="cocktailItem in cocktailDataList"
        v-bind:key="cocktailItem.id">
        {{ cocktailItem.name }}の値段は{{ cocktailItem.price }}円です
      </li>
    </ul>
  </section>
  <section>
    値段が1500円のカクテルリスト
    <ul>
      <li
        v-for="cocktailItem in cocktail1500"
        v-bind:key="'cocktail1500' + cocktailItem.id">
        {{ cocktailItem.name }}の値段は{{ cocktailItem.price }}円
      </li>
    </ul>
  </section>
</template>
```
<br>

### 6.3.2 配列のデータ操作 
```ts
vueChapter6.3/listchange-array/listchange-array/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const cocktailListInit: string[] = ["ホワイトレディ", "ブルーハワイ", "ニューヨーク"];
  const cocktailList = ref(cocktailListInit);
  const changeCocktailList = (): void => {
    cocktailList.value = ["バラライカ", "XYZ", "マンハッタン"];
    };
  const addCocktailList = (): void => {
    cocktailList.value.push("ブルームーン");
    };
  const deleteFromCocktailList = (): void => {
    cocktailList.value.pop();
    };
</script>

<template>
  <ul>
    <li
      v-for="(cocktailName, index) in cocktailList"
      v-bind:key="cocktailName">
      {{ cocktailName }}(インデックス{{ index }})
    </li>
  </ul>
  <p>
    CocktailListを
    <button v-on:click="changeCocktailList">変更</button>
  </p>
  <p>
    CocktailListの末尾に「ブルームーン」を
    <button v-on:click="addCocktailList">追加</button>
  </p>
  <p>
    CocktailListから末尾の要素を
    <button v-on:click="deleteFromCocktailList">削除</button>
  </p>
</template>
```
<br>

### 6.3.3 Mapのデータ操作
```ts
vueChapter6.3/listchange-map/listchange-map/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const cocktailListInit = new Map<number, string>();
  cocktailListInit.set(2345, "ホワイトレディ");
  cocktailListInit.set(4412, "ブルーハワイ");
  cocktailListInit.set(6792, "ニューヨーク");
  const cocktailList = ref(cocktailListInit);
  const changeCocktailList = (): void => {
    cocktailList.value.clear();
    cocktailList.value.set(3416, "バラライカ");
    cocktailList.value.set(5517, "XYZ");
    cocktailList.value.set(7415, "マンハッタン");
    };
  const addCocktailList = (): void => {
    cocktailList.value.set(8894, "ブルームーン");
    };
  const deleteFromCocktailList = (): void => {
    cocktailList.value.delete(5517);
    };
</script>

<template>
  <ul>
    <li
      v-for="[id, cocktailName] in cocktailList"
      v-bind:key="id">
      IDが{{ id }}のカクテルは{{ cocktailName }}
    </li>
  </ul>
  <p>
    CocktailListを
    <button v-on:click="changeCocktailList">変更</button>
  </p>
  <p>
    CocktailListの末尾に「ブルームーン」を
    <button v-on:click="addCocktailList">追加</button>
  </p>
  <p>
    CocktailListから5517の要素を
    <button v-on:click="deleteFromCocktailList">削除</button>
  </p>
</template>
```
<br>

### 6.3.4 オブジェクト内のデータ変更
```ts
vueChapter6.3/listchange-obj/listchange-obj/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const whiteLadyInit: {
    id: number;
    name: string;
    price: number;
    recipe: string;
    } = {
      id: 2345,
      name: "ホワイトレディ",
      price: 1200,
      recipe: "ジン30ml+コワントロー15ml+レモン果汁15ml"
    };
  const whiteLady = ref(whiteLadyInit);
  const changeWhiteLadyPrice = (): void => {
    whiteLady.value.price = 1500;
    };
</script>

<template>
  <dl>
    <template
      v-for="(value, key) in whiteLady"
      v-bind:key="key">
      <dt>{{ key }}</dt>
      <dd>{{ value }}</dd>
    </template>
  </dl>
  <p>
    価格を1500円に
    <button v-on:click="changeWhiteLadyPrice">変更</button>
  </p>
</template>
```
<br>

### 6.3.5 リストデータ内のオブジェクトの変更
```ts
vueChapter6.3/listchange-mapobj/listchange-mapobj/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';

  const cocktailDataListInit = new Map<number, Cocktail>();
  cocktailDataListInit.set(2345, {id: 2345, name: "ホワイトレディ", price: 1200});
  cocktailDataListInit.set(4412, {id: 4412, name: "ブルーハワイ", price: 1500});
  cocktailDataListInit.set(6792, {id: 6792, name: "ニューヨーク", price: 1100});
  cocktailDataListInit.set(8429, {id: 8429, name: "マティーニ", price: 1500});
  const cocktailDataList = ref(cocktailDataListInit);
  const cocktail1500 = computed(
    //算出関数。戻り値の方はMap。
    (): Map<number, Cocktail> => {
      //絞り込んだ要素を格納する新しいMapを用意
      const newList = new Map<number, Cocktail>();
      //cocktailDataList内のMapをループ処理
      cocktailDataList.value.forEach(
        //ループの各処理内容を表す関数
        (value: Cocktail, key: number): void => {
          //カクテルの価格が1500円ならnewListに登録
          if(value.price == 1500) {
            newList.set(key, value);
            }
          }
        );
      //絞り込んだ新しいMapを戻り値とする
      return newList;
      }
    );
  const changeWhiteLadyPriceInList = (): void => {
    const whiteLady = cocktailDataList.value.get(2345) as Cocktail;
    whiteLady.price = 1500;
    };
  
  interface Cocktail {
    id: number;
    name: string;
    price: number;
    }
</script>

<template>
  <section>
    全てのカクテルリスト
    <ul>
      <li
        v-for="[id, cocktailItem] in cocktailDataList"
        v-bind:key="id">
        {{ cocktailItem.name }}の値段は{{ cocktailItem.price }}円です
      </li>
    </ul>
  </section>
  <section>
    値段が1500円のカクテルリスト
    <ul>
      <li
        v-for="[id, cocktailItem] in cocktail1500"
        v-bind:key="'cocktail1500'+ id">
        {{ cocktailItem.name }}の値段は{{ cocktailItem.price }}円
      </li>
    </ul>
  </section>
  <p>
    CocktailDataList内のホワイトレディの価格を1500円に
    <button v-on:click="changeWhiteLadyPriceInList">変更</button>
  </p>
</template>
```
<br>