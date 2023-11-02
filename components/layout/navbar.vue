import type { hydrate } from 'vue'; import type { NuxtLink } from
'#build/components';
<template>
  <nav class="flex justify-between px-6 py-6">
    <NuxtLink to="/">DJME</NuxtLink>

    <div class="flex gap-4">
      <NuxtLink to="/register">Registrer</NuxtLink>
      <NuxtLink to="/login">Login</NuxtLink>
      <NuxtLink to="/profile">Profile</NuxtLink>
    </div>
  </nav>
  <hr />
</template>
