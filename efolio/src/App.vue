<script setup>
import MyName from "./components/MyName.vue";
</script>

<template>
  <div class="app">
    <!-- existing welcome content -->
    <h1>Welcome to Vue</h1>

    <!-- new component output -->
    <MyName />
  </div>
</template>

<style scoped>
.app {
  padding: 1.5rem;
  font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
}
</style>
