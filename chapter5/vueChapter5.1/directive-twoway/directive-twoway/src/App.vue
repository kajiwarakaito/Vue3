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

<style scoped>

</style>
