<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <nav>
      <RouterLink to="/"></RouterLink>
      <RouterLink to="/predict"></RouterLink>
    </nav>
  </header>

  <RouterView />
</template>
