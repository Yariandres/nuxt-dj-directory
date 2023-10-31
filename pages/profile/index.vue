<script setup lang="ts">
// definePageMeta({
//   middleware: ['auth'],
// });

const user = useSupabaseUser();
const client = useSupabaseClient();
const router = useRouter();

async function logout() {
  try {
    const { error } = await client.auth.signOut();
    if (error) throw error;
    router.push('/login');
  } catch (error) {
    console.log(error.message);
  }
}
</script>
<template>
  <main>
    <h1 class="text-xl mb-2">Email: {{ user.email }}</h1>
    <button
      @click="logout"
      class="border rounded-lg p-4 bg-green-600"
      type="button"
    >
      Logout
    </button>
  </main>
</template>
