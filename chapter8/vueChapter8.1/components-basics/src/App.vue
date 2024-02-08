<script setup lang="ts">
  import OnSection from "./components/OneSection.vue";
</script>

<template>
  <h1>コンポーネント基礎</h1>
  <section>
    <h2>コンポーネント1個</h2>
    <OnSection />
  </section>
  <section>
    <h2>コンポーネントが複数</h2>
    <OnSection />
    <OnSection />
    <OnSection />
  </section>
</template>

<style>
  section {
    border: blue 1px solid;
    margin: 10px;
  }
</style>