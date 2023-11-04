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
  } catch (error: any) {
    errorMsg.value = error.message;
  }
}
</script>
<template>
  <main class="bg-slate-900">
    <section class="h-screen flex justify-center items-center">
      <div class="flex flex-col bg-white p-6 rounded-md shadow-sm gap-4">
        <h1 class="text-2xl p-4 text-center text-slate-600">
          Register and create you profile
        </h1>

        <form @submit.prevent="signup">
          <div class="flex flex-col gap-4">
            <div>
              <label for="email" class="block text-slate-500">Email</label>
              <input
                v-model="email"
                class="border py-2 px-3 rounded-lg w-full bg-slate-400"
                type="text"
                id="email"
                placeholder="email@example.com"
              />
            </div>
            <div>
              <label for="password" class="block text-slate-500"
                >Password</label
              >
              <input
                v-model="password"
                class="border py-2 px-3 rounded-lg w-full"
                type="password"
                id="password"
                placeholder="****"
              />
            </div>
            <p class="text-red-700">{{ errorMsg }}</p>
            <p class="text-green-700">{{ successMsg }}</p>
          </div>
          <button
            v-if="!successMsg"
            type="submit"
            class="border py-2 px-3 rounded-lg w-full bg-green-900 hover:bg-green-700 text-white hover:shadow-md"
          >
            Register
          </button>
        </form>
      </div>
    </section>
  </main>
</template>
