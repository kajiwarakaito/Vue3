<script setup lang="ts">
  import { ref } from 'vue';
  import OnSection from "./components/OneSection.vue";

  const taro = ref("田中太郎");
  const jiro = ref("鈴木二郎");
</script>

<template>
  <section>
    <h2>Slotの利用</h2>
    <OnSection v-bind:name="taro">
      <p>連絡がつきません</p>
    </OnSection>
    <OnSection v-bind:name="jiro"/>
  </section>
</template>