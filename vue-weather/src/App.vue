<template>
  <div class="flex flex-col min-h-screen font-Nunito bg-weather-primary">
    <SiteNav />
    <RouterView />
  </div>
</template>

<script setup>
import { RouterView } from "vue-router";
import SiteNav from "./components/SiteNav.vue";
</script>

<style lang="scss" scoped></style>
