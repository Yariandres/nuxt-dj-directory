<script setup lang="ts">
const client = useSupabaseClient();
const router = useRouter();

const email = ref<string>();
const password = ref<string | null>(null);
const errorMsg = ref<string | null>(null);

async function signIn() {
  try {
    const { error } = await client.auth.signInWithPassword({
      email: email.value,
      password: password.value,
    });
    if (error) throw error;
    router.push('/profile');
  } catch (error: any) {
    errorMsg.value = error.message;
  }
}
</script>
<template>
  <main class="bg-slate-300">
    <section class="h-screen flex justify-center items-center">
      <h1>Login</h1>
      <div class="back">
        <form @submit.prevent="signIn">
          <div>
            <label for="email" class="block">Email</label>
            <input
              v-model="email"
              class="border"
              type="text"
              id="email"
              placeholder="email@example.com"
            />
          </div>
          <div>
            <label for="password" class="block">Password</label>
            <input
              v-model="password"
              class="border"
              type="password"
              id="password"
              placeholder="****"
            />
          </div>
          <p class="text-red-700">{{ errorMsg }}</p>
          <button type="submit" class="border p-4 rounded-lg">Submit</button>
        </form>
      </div>
    </section>
  </main>
</template>
