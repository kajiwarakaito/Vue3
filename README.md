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

hrefとはちがいdisabledやreadonlyには属性値がない
```ts
<script setup lang="ts">
  import {ref} from "vue";
  const inSendButtonDisabled = ref(true);
</script>

<template>
  <p><button type="button" v-bind:disabled="inSendButtonDisabled">送信</button></p>
</template>
```
inSendButtonDisabledのtrue/falseのbooleanがtrueの場合はレンダリング結果に属性が付与される  
<br>

```ts
<script setup lang="ts">
  import {ref} from "vue";
  const widthOrHeight = ref("height");
  const widthOrHeightValue = ref(100);
</script>

<template>
  <p>
    <img alt="VueLogo" src="./assets/logo.svg" v-bind:[widthOrHeight]="widthOrHeightValue">
  </p>
</template>
```
のようにバインドする属性をテンプレート変数にすることができる
レンダリング結果
`<img alt="VueLogo" src="/src/assets/logo.svg" height="100">`  
<br>

まとめてバインド
```ts
<script setup lang="ts">
  import {ref} from "vue";
  const imgAttributes = ref({
    src: "/images/logo.srg",
    alt: "Vueのロゴ",
    width: 75,
    height: 75
    });
</script>

<template>
  <p><img v-bind="imgAttributes"></p>
  <p><img v-bind="imgAttributes" title="ロゴです！"></p>
  <p><img v-bind="imgAttributes" alt="ロゴです！"></p>
</template>
```
<br>

これでstyle属性にバインディングできる
```ts
<script setup lang="ts">
  import {ref, computed} from "vue";

  const msg = ref("こんにちは！世界");
  const msgTextRed = ref("red");
  const msgTextColor = ref("white");
  const msgBgColor = ref("black");

  const msgStyles = ref({
    color: "white",
    backgroundColor: "black"
    });
  const msgStyles2 = ref({
    fontSize: "24pt"
    });
  const msgStyles3 = ref({
    color: "pink",
    fontSize: "24pt"
    });
  
  const textSize = computed(
    (): string => {
      const size = Math.round(Math.random() * 25) + 10;
      return `${size}pt`;
      }
    );
</script>

<template>
  <p v-bind:style="{color: msgTextRed}">
    {{ msg }}
  </p>
  <p v-bind:style="{color: 'pink'}">
    {{ msg }}
  </p>

  <p v-bind:style="{fontSize: textSize}">
    {{ msg }}
  </p>
  
  <p v-bind:style="{color: msgTextColor, backgroundColor: msgBgColor}">
    {{ msg }}
  </p>
  <p v-bind:style="{color: msgTextColor, 'backgroundColor': msgBgColor}">
    {{ msg }}
  </p>
  
  <p v-bind:style="msgStyles">
    {{ msg }}
  </p>
  <p v-bind:style="[msgStyles, msgStyles2]">
    {{ msg }}
  </p>
  <p v-bind:style="[msgStyles, msgStyles3]">
    {{ msg }}
  </p>
  <p v-bind:style="[msgStyles3, msgStyles]">
    {{ msg }}
  </p>
</template>
```
<br>

これでclass属性にデータバインディング
```ts
<script setup lang="ts">
import {ref, computed} from "vue";

const msg = ref("こんにちは！世界");
const isTextColorRed = ref(true);
const isBgColorBlue = ref(false);

const styles = ref({
  textColorRed: false,
  bgColorBlue: true
  });

const computedStyles = computed(
  (): {textColorRed: boolean; bgColorBlue: boolean;} => {
    //乱数を利用して0か1を生成(textColorRed用)
    const randText = Math.round(Math.random());
    //textColorRedプロパティの値を表す変数をtrueで用意
    let textColorFlg = true;
    //発生した乱数が0ならばfalseに変更
    if(randText == 0) {
      textColorFlg = false;
      }
    
    //乱数を利用して0か1を生成(bgColorBlue用)
    const randBg = Math.round(Math.random());
    //bgColorBlueプロパティの値を表す変数をtrueで用意
    let bgColorFlg = true;
    //発生した乱数が0ならばfalseに変更
    if(randBg == 0) {
      bgColorFlg = false;
      }
    //それぞれのプロパティの値をオブジェクトにして返す
    return {
      textColorRed: textColorFlg,
      bgColorBlue: bgColorFlg
      };
    }
  );
</script>

<template>
  <p v-bind:class="{textColorRed: true, bgColorBlue: true}">
    {{ msg }}
  </p>
  <p v-bind:class="{textColorRed: isTextColorRed, bgColorBlue: isBgColorBlue}">
    {{ msg }}
  </p>

  <p v-bind:class="{textColorPink: true}">
    {{ msg }}
  </p>
  <p v-bind:class="{'textColorPink': true}">
    {{ msg }}
  </p>
  
  <p class="textSize24" v-bind:class="{textColorRed: isTextColorRed, bgColorBlue: isBgColorBlue}">
    {{ msg }}
  </p>
  <p class="textSize24" v-bind:class="styles">
    {{ msg }}
  </p>
  
  <p v-bind:class="computedStyles">
    {{ msg }}
  </p>
</template>

<style scoped>
  .textColorRed {
    color: red;
  }
  .text-color-pink {
    color: pink;
  }
  .bgColorBlue {
    background-color: blue;
  }
  .textSize24 {
    font-size: 24px;
  }
</style>
```

## 4.2 イベントのディレクティブ
