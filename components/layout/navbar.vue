<script lang="ts" setup>
const user = useSupabaseUser();
</script>

<template>
  <nav class="flex justify-between px-6 py-6 bg-slate-800">
    <NuxtLink
      class="text-white text-2xl text-transparent bg-clip-text bg-gradient-to-r from-indigo-600 to-red-600"
      to="/"
      >DJME</NuxtLink
    >

    <div class="flex gap-4">
      <NuxtLink v-if="!user" class="text-white" to="/register"
        >Registrer</NuxtLink
      >
      <NuxtLink v-if="!user" class="text-white" to="/login">Login</NuxtLink>
      <NuxtLink v-if="user" class="text-white" to="/profile">Profile</NuxtLink>
    </div>
  </nav>
</template>
