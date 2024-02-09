<script setup lang="ts">
  import OneSection from "./components/OneSection.vue";
</script>

<template>
  <section>
    <OneSection>
      <template v-slot:default="slotProps">
      <dl>
        <dt>名前</dt>
        <dd>{{ slotProps.memberInfo.name }}</dd>
        <dt>状況</dt>
        <dd>{{ slotProps.memberInfo.state }}</dd>
      </dl>
    </template>
    </OneSection>
  </section>
</template>