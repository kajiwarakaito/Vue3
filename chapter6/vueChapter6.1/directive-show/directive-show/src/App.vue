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