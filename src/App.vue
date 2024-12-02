<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <RouterView />
</template>

<script>
export default {
  watch: {
    // Watch for route changes
    $route(to, from) {
      const jwt = localStorage.getItem('jwt');

      // If no JWT and on /login or / (root), redirect to /login
      if (!jwt && (to.path === '/' || to.path === '/login')) {
        this.$router.push('/login');
      } 
      // If JWT exists and on /login or / (root), redirect to /welcome
      else if (jwt && (to.path === '/' || to.path === '/login')) {
        this.$router.push('/welcome');
      }
    }
  }
}
</script>
<style scoped>
</style>
