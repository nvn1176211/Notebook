<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HeaderComponent from './components/Header.vue'

import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <HeaderComponent />
  <div class="container pt-10">
    <RouterLink to="/tag" class="text-decoration-none">Tag Creation</RouterLink>
    <RouterView />
  </div>
</template>