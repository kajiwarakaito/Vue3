<script setup lang="ts">
  import {ref} from "vue";
  const inSendButtonDisabled = ref(true);
</script>

<template>
  <p><button type="button" v-bind:disabled="inSendButtonDisabled">送信</button></p>
</template>

<style scoped>

</style>
