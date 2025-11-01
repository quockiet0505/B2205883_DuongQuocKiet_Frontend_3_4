<script>
import AppHeader from '@/components/AppHeader.vue';
export default {
  components: {
    AppHeader,
  },
};
</script>

<template>
  <div id="app">
    <AppHeader />
    <div class="container mt-3">
      <router-view />
    </div>
  </div>
</template>

<style>

#app {
  display: flex;
  flex-direction: column; 
  min-height: 100vh; 
}

/* giữ style cũ */
.page {
  max-width: 400px;
  margin: auto;
}
</style>
