<script setup lang="ts">
  import {ref, computed, reactive} from "vue";

  // 半径の初期値を乱数で取得
  const radiusInit = Math.round(Math.random() * 10);
  // 円周率のテンプレート変数を用意
  const PI = ref(3.14);
  // 半径のテンプレート変数を用意
  const radius = ref(radiusInit);
  // 円の面積の算出プロパティを用意
  const area = computed(
    (): number => {
      return radius.value * radius.value * PI.value;
      }
    //(): number => radius.value * radius.value * PI.valueでも可
    );
  // または
  // const area = computed(
  //   function(): number {
  //     return radius.value * radius.value * PI.value;
  //     }
  //   );
  // でも可
  
  // 半径のテンプレート変数に新しい乱数を1秒ごとに格納
  setInterval(
    (): void => {
      radius.value = Math.round(Math.random() * 10);
      }, 1000

  );

  
// reactive関数を用いてこういう書き方もできる------*1*
// import {reactive, computed} from "vue";

// リアクティブなテンプレート関数をまとめて用意
// const data = reactive({  <------------------dataをreactive関数としてまとめて定義している
//   PI: 3.14,
//   radius: Math.round(Math.random()*10)
//   });
// 円の面積の算出プロパティを用意
// const area = computed(
//   (): number => {
//     return data.radius * data.radius * data.PI;  <------dataに宣言名を付けることで指定
//     }
//   );
// 半径のテンプレート変数に新しい乱数を1秒ごとに格納
// setInterval(
//   (): void => {
//     data.radius = Math.round(Math.random() * 10);  <------data.radiusを1秒で更新
//     }, 1000
// );

</script>

<template>
  <p>半径{{ radius }}の円の面積を円周率{{ PI }}で計算すると、{{ area }}</p>

  <!-- 
  *1*の場合
  <p>半径{{ data.radius }}の円の面積を円周率{{ data.PI }}で計算すると、{{ area }}</p>
  -->


</template>

<style scoped>

</style>
