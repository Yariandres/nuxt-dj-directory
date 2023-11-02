<script lang="ts" setup>
const user = useSupabaseUser();
</script>

<template>
  <nav class="flex justify-between px-6 py-6">
    <NuxtLink to="/">DJME</NuxtLink>

    <div class="flex gap-4">
      <NuxtLink v-if="!user" to="/register">Registrer</NuxtLink>
      <NuxtLink v-if="!user" to="/login">Login</NuxtLink>
      <NuxtLink v-if="user" to="/profile">Profile</NuxtLink>
    </div>
  </nav>
  <hr />
</template>
