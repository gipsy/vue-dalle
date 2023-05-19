<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { logo } from './assets'
</script>

<template>
  <div>
    <header>
      <RouterLink to="/">
        <img :src="logo" alt="" class="logo" />
      </RouterLink>
      <RouterLink to="/create-post" class="link">
        Create
      </RouterLink>
    </header>
    <main>
      <RouterView />
    </main>
  </div>
</template>

