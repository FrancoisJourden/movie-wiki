<script setup lang="ts">
import { RouterView } from 'vue-router'
</script>

<template>
  <header class="bg-slate-700 text-white flex p-2">
    <h1 class="font-bold whitespace-nowrap my-auto hover:cursor-pointer" @click="this.$router.push('/')">The Movie Wiki</h1>
    <span class="w-full"/>
    <input type="search" placeholder="Search here..." class="p-1 rounded my-auto text-black" />
  </header>

  <RouterView />

  <footer class="bg-slate-700 text-white p-2">
    Copyright {{ new Date().getFullYear() }} @ François Jourden
  </footer>
</template>