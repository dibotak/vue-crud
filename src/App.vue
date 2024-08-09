<script setup>
import { RouterLink, RouterView } from 'vue-router'
import '@/style.css'
</script>

<template>
  <header>
    <div class="shadow-md w-full flex items-center">
      <div class="px-4 font-bold">
        <RouterLink to="/">CRUD</RouterLink>
      </div>
      <nav class="w-full flex justify-center space-x-1">
        <RouterLink
          to="/"
          class="p-4 hover:underline hover:bg-blue-100"
          active-class="bg-blue-200"
        >
          Home
        </RouterLink>
        <RouterLink
          to="/about"
          class="p-4 hover:underline hover:bg-blue-100"
          active-class="bg-blue-200"
        >
          About
        </RouterLink>
        <RouterLink
          to="/todo"
          class="p-4 hover:underline hover:bg-blue-100"
          active-class="bg-blue-200"
        >
          ToDo
        </RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
</style>
