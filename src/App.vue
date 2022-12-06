<script setup lang="ts">
import FirstSection from './components/FirstSection.vue';
import SecondSection from './components/SecondSection.vue';
import ThirdSection from './components/ThirdSection.vue';
</script>

<template>
  <div class="app">
    <first-section></first-section>
    <second-section></second-section>
    <third-section></third-section>
  </div>
</template>

<style lang="scss" scoped>
.app {
  background: #e36c3f;
  padding-bottom: 64px;
}
</style>
