<script setup lang="ts">
  interface Props {
    name: string;
  }
  defineProps<Props>();
</script>

<template>
  <section class="box">
    <h1>{{ name }}さんの状況</h1>
    <slot>
      <p>{{name}}さんは問題ありません</p>
    </slot>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
  }
</style>
