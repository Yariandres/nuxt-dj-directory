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

    if (error) throw error;
    successMsg.value = 'Check you email to confirm your account';
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
          <p>{{ errorMsg }}</p>
          <p>{{ successMsg }}</p>
        </form>
      </div>
    </section>
  </main>
</template>
