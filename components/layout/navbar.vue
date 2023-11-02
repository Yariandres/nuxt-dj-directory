<script lang="ts" setup>
const user = useSupabaseUser();
console.log(user);
</script>

<template>
  <nav class="flex justify-between px-6 py-6">
    <NuxtLink to="/">DJME</NuxtLink>

    <div class="flex gap-4">
      <NuxtLink to="/register">Registrer</NuxtLink>
      <NuxtLink to="/login">Login</NuxtLink>
      <NuxtLink v-if="user" to="/profile">Profile</NuxtLink>
    </div>
  </nav>
  <hr />
</template>
