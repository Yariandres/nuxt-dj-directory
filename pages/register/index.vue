<script setup lang="ts">
import { ref } from 'vue';
const client = useSupabaseClient();

const email = ref<string>('');
const password = ref<string | null>(null);
const errorMsg = ref<string | null>(null);
const successMsg = ref<string | null>(null);

async function signup() {
  try {
    const { data, error } = await client.auth.signUp({
      email: email.value,
      password: password.value,
    });
    successMsg.value = 'Check your email to confirm your account';

    if (error) throw error;
  } catch (error) {
    errorMsg.value = error.message;
  }
}
</script>
<template>
  <main>
    <section class="h-screen flex justify-center items-center">
      <div class="">
        <h1>Register</h1>
        <form @submit.prevent="signup">
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
          <p class="text-green-700">{{ successMsg }}</p>
          <button type="submit" class="border p-4 rounded-lg">Submit</button>
        </form>
      </div>
    </section>
  </main>
</template>
