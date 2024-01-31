<script setup lang="ts">
  import {ref} from "vue";
  const name1 = ref("田中太郎");

  const yourName = "鈴木次郎";
  const name2 = ref(yourName);
</script>

<template>
  <h1>こんにちは！{{ name1 }}さん！</h1>
  <h1>さようなら！{{ name2 }}さん！</h1>
</template>

<style scoped>

</style>
