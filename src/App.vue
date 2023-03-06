<script setup lang="ts">
import { RouterView } from 'vue-router'
</script>

<template>
  <header class="dark:bg-slate-700 flex p-2">
    <h1 class="font-bold whitespace-nowrap my-auto hover:cursor-pointer" @click="this.$router.push('/')">The Movie Wiki</h1>
    <span class="w-full"/>
    <input type="search" placeholder="Search here..." class="p-1 rounded my-auto text-black" />
  </header>

  <RouterView />
</template>