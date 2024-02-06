## 「Vue3 フロントエンド開発の教科書」7章  
## 7.1 ウォッチャー
### 7.1.1 算出プロパティの役割
```ts
vueChapter7.1/change-cocktail/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';

  const cocktailDataListInit = new Map<number, Cocktail>();
  cocktailDataListInit.set(1, {id: 1, name: "ホワイトレディ", price: 1200});
  cocktailDataListInit.set(2, {id: 2, name: "ブルーハワイ", price: 1500});
  cocktailDataListInit.set(3, {id: 3, name: "ニューヨーク", price: 1100});
  cocktailDataListInit.set(4, {id: 4, name: "マティーニ", price: 1500});

  const cocktailNo = ref(1);
  const priceMsg = computed(
    (): string => {
      const cocktail = cocktailDataListInit.get(cocktailNo.value);
      let msg = "該当のカクテルはありません";
      if(cocktail != undefined) {
        msg = `該当するカクテルは ${cocktail.name} で、価格は ${cocktail.price} 円です。`
        }
        return msg;
      }
    );
  
  setInterval(
    (): void => {
      cocktailNo.value = Math.round(Math.random() * 3) + 1;
      },1000
    );

    interface Cocktail {
      id: number;
      name: string;
      price: number;
      }
</script>

<template>
  <p>現在のカクテル番号: {{ cocktailNo }}</p>
  <p>{{ priceMsg }}</p>
</template>
```
<br>

### 7.1.2 リアクティブ変数の変化を監視するwatchEffect()
```ts
vueChapter7.1/watcheffect/src/App.vue

<script setup lang="ts">
  import { ref, watchEffect } from 'vue';

  const cocktailNo = ref(1);
  const priceMsg = ref("");
  watchEffect(
    (): void => {
      priceMsg.value = getCocktailInfo(cocktailNo.value);
      }
    );

  setInterval(
    (): void => {
      cocktailNo.value = Math.round(Math.random() * 3) + 1;
      },1000
    );

  interface Cocktail {
    id: number;
    name: string;
    price: number;
    }

  function getCocktailInfo(cocktailNo: number): string {
    const cocktailDataListInit = new Map<number, Cocktail>();
    cocktailDataListInit.set(1, {id: 1, name: "ホワイトレディ", price: 1200});
    cocktailDataListInit.set(2, {id: 2, name: "ブルーハワイ", price: 1500});
    cocktailDataListInit.set(3, {id: 3, name: "ニューヨーク", price: 1100});
    cocktailDataListInit.set(4, {id: 4, name: "マティーニ", price: 1500});

    const cocktail = cocktailDataListInit.get(cocktailNo);
      let msg = "該当のカクテルはありません";
      if(cocktail != undefined) {
        msg = `該当するカクテルは ${cocktail.name} で、価格は ${cocktail.price} 円です。`
        }
        return msg;
    }
</script>

<template>
  <p>現在のカクテル番号: {{ cocktailNo }}</p>
  <p>{{ priceMsg }}</p>
</template>
```
<br>

### 7.1.3 監視対象を明示するwatch()
```ts
vueChapter7.1/watch/src/App.vue

<script setup lang="ts">
  import { ref, watch } from 'vue';

  const cocktailNo = ref(1);
  const priceMsg = ref("");
  watch(cocktailNo, 
    (): void => {
      priceMsg.value = getCocktailInfo(cocktailNo.value);
      },
      {immediate: true}
    );

  setInterval(
    (): void => {
      cocktailNo.value = Math.round(Math.random() * 3) + 1;
      },1000
    );

  interface Cocktail {
    id: number;
    name: string;
    price: number;
    }

  function getCocktailInfo(cocktailNo: number): string {
    const cocktailDataListInit = new Map<number, Cocktail>();
    cocktailDataListInit.set(1, {id: 1, name: "ホワイトレディ", price: 1200});
    cocktailDataListInit.set(2, {id: 2, name: "ブルーハワイ", price: 1500});
    cocktailDataListInit.set(3, {id: 3, name: "ニューヨーク", price: 1100});
    cocktailDataListInit.set(4, {id: 4, name: "マティーニ", price: 1500});

    const cocktail = cocktailDataListInit.get(cocktailNo);
      let msg = "該当のカクテルはありません";
      if(cocktail != undefined) {
        msg = `該当するカクテルは ${cocktail.name} で、価格は ${cocktail.price} 円です。`
        }
        return msg;
    }
</script>

<template>
  <p>現在のカクテル番号: {{ cocktailNo }}</p>
  <p>{{ priceMsg }}</p>
</template>
```
<br>

### 7.1.4 即時実行のwatch()
```ts
vueChapter7.1/watch/src/App.vue

<script setup lang="ts">
  import { ref, watch } from 'vue';

  const cocktailNo = ref(1);
  const priceMsg = ref("");
  watch(cocktailNo, 
    (): void => {
      priceMsg.value = getCocktailInfo(cocktailNo.value);
      },
      {immediate: true}
    );

  setInterval(
    (): void => {
      cocktailNo.value = Math.round(Math.random() * 3) + 1;
      },1000
    );

  interface Cocktail {
    id: number;
    name: string;
    price: number;
    }

  function getCocktailInfo(cocktailNo: number): string {
    const cocktailDataListInit = new Map<number, Cocktail>();
    cocktailDataListInit.set(1, {id: 1, name: "ホワイトレディ", price: 1200});
    cocktailDataListInit.set(2, {id: 2, name: "ブルーハワイ", price: 1500});
    cocktailDataListInit.set(3, {id: 3, name: "ニューヨーク", price: 1100});
    cocktailDataListInit.set(4, {id: 4, name: "マティーニ", price: 1500});

    const cocktail = cocktailDataListInit.get(cocktailNo);
      let msg = "該当のカクテルはありません";
      if(cocktail != undefined) {
        msg = `該当するカクテルは ${cocktail.name} で、価格は ${cocktail.price} 円です。`
        }
        return msg;
    }
</script>

<template>
  <p>現在のカクテル番号: {{ cocktailNo }}</p>
  <p>{{ priceMsg }}</p>
</template>
```
<br>

### 7.1.5 watch()における変更前後の値の利用
```ts
vueChapter7.1/watch-oldnew/src/App.vue

<script setup lang="ts">
  import { ref, watch } from 'vue';

  const cocktailNo = ref(1);
  const priceMsg = ref("");
  watch(cocktailNo,
    (newVal: number, oldVal: number): void => {
      let msg = "前のカクテル：";
      msg += getCocktailInfo(oldVal);
      msg += "現在のカクテル：";
      msg += getCocktailInfo(newVal);
      priceMsg.value = msg;
      }
    );

  setInterval(
    (): void => {
      cocktailNo.value = Math.round(Math.random() * 3) + 1;
      },1000
    );

  interface Cocktail {
    id: number;
    name: string;
    price: number;
    }

  function getCocktailInfo(cocktailNo: number): string {
    const cocktailDataListInit = new Map<number, Cocktail>();
    cocktailDataListInit.set(1, {id: 1, name: "ホワイトレディ", price: 1200});
    cocktailDataListInit.set(2, {id: 2, name: "ブルーハワイ", price: 1500});
    cocktailDataListInit.set(3, {id: 3, name: "ニューヨーク", price: 1100});
    cocktailDataListInit.set(4, {id: 4, name: "マティーニ", price: 1500});

    const cocktail = cocktailDataListInit.get(cocktailNo);
      let msg = "該当のカクテルはありません";
      if(cocktail != undefined) {
        msg = `該当するカクテルは ${cocktail.name} で、価格は ${cocktail.price} 円です。`
        }
        return msg;
    }
</script>

<template>
  <p>現在のカクテル番号: {{ cocktailNo }}</p>
  <p>{{ priceMsg }}</p>
</template>
```

## 7.2 ライフサイクルフック
### 7.2.2 ライフサイクルフックの具体例
```ts
vueChapter7.2/lifecycle-hooks/src/App.vue

<script setup lang="ts">
  import { ref, computed, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onRenderTracked,
            onRenderTriggered } from 'vue';
  import type { DebuggerEvent } from 'vue';

  const heightInit = Math.round(Math.random() * 10);
  const widthInit = Math.round(Math.random() * 10);
  const height = ref(heightInit);
  const width = ref(widthInit);
  const area = computed(
    (): number => {
      return height.value * width.value;
      }
    );
  const change = (): void => {
    height.value = Math.round(Math.random() * 10);
    width.value = Math.round(Math.random() * 10);
    }
  onBeforeMount(
    (): void => {
      console.log(`beforMount called: ${height.value} * ${width.value}`);
      }
    );
  onMounted(
    (): void => {
      console.log(`mounted called: ${height.value} * ${width.value}`);
      }
    );
  onBeforeUpdate(
    (): void => {
      console.log(`beforUpdate called: ${height.value} * ${width.value}`);
      }
    );
  onUpdated(
    (): void => {
      console.log(`updated called: ${height.value} * ${width.value}`);
      }
    );
  onRenderTracked(
    (event: DebuggerEvent): void => {
      console.log(`renderTriggered called: ${height.value} * ${width.value}`);
      console.log(event);
      }
    );
  onRenderTriggered(
    (event: DebuggerEvent): void => {
      console.log(`renderTriggered called: ${height.value} * ${width.value}`);
      console.log(event);
      }
    );
</script>

<template>
  <p>縦が{{ height }}、横が{{ width }}の長方形の面積は{{ area }}</p>
  <button v-on:click="change">値を変更</button>
</template>
```
<br>

## 7.3 script setupの本当の姿
### 7.3.1 defineComponentとsetup
```ts
vueChapter7.3/change-cocktail2/src/App.vue

<script lang="ts">
  import { defineComponent, ref, computed } from 'vue';

  export default defineComponent({
    name: "App",
    setup() {
      const cocktailDataListInit = new Map<number, Cocktail>();
      cocktailDataListInit.set(1, {id: 1, name: "ホワイトレディ", price: 1200});
      cocktailDataListInit.set(2, {id: 2, name: "ブルーハワイ", price: 1500});
      cocktailDataListInit.set(3, {id: 3, name: "ニューヨーク", price: 1100});
      cocktailDataListInit.set(4, {id: 4, name: "マティーニ", price: 1500});
      const cocktailNo = ref(1);
      const priceMsg = computed(
        (): string => {
          const cocktail = cocktailDataListInit.get(cocktailNo.value);
          let msg = "該当のカクテルはありません";
          if(cocktail != undefined) {
            msg = `該当するカクテルは ${cocktail.name} で、価格は ${cocktail.price} 円です。`
            }
            return msg;
          }
        );
      setInterval(
        (): void => {
          cocktailNo.value = Math.round(Math.random() * 3) + 1;
          },1000
        );
        return {
          cocktailNo,
          priceMsg
          }
      }
    });
  
  interface Cocktail {
    id: number;
    name: string;
    price: number;
    }
</script>

<template>
  <p>現在のカクテル番号: {{ cocktailNo }}</p>
  <p>{{ priceMsg }}</p>
</template>
```
<br>

### 7.3.2 setupとreactiveとtoRefs
```ts
vueChapter7.3/reactiveFunctionSetup/src/App.vue

<script lang="ts">
  import { defineComponent, reactive, computed } from 'vue';

  export default defineComponent({
    name: "App",
    setup() {
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
        (): void => {
          data.radius = Math.round(Math.random() * 10);
          },1000
        );
      return {
        data,
        area
        }
      }
    });
</script>

<template>
  <p>半径{{ data.radius }}の円の面積を円周率{{ data.PI }}で計算すると、{{ area }}</p>
</template>
```

## 7.4 Options API
### 7.4.2 Options APIの基本構文
```ts
vueChapter7.4/optionsapi/src/App.vue

<script lang="ts">
  import { defineComponent } from 'vue';

  export default defineComponent({
    name: "App",
    data() {
      return {
        height: Math.round(Math.random() * 10),
        width: Math.round(Math.random() * 10)
        }
      },
      computed: {
        area(): number {
          return this.height * this.width;
          }
        },
        methods: {
          change(): void {
            this.height = Math.round(Math.random() * 10);
            this.width = Math.round(Math.random() * 10);
            }
          }
    });
</script>
<template>
  <p>縦が{{ height }}、横が{{ width }}の長方形の面積は{{ area }}</p>
  <button v-on:click="change"></button>
</template>
```