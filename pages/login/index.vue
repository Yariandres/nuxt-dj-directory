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
  <main class="bg-blue-900">
    <section class="h-screen flex justify-center items-center">
      <div class="flex flex-col bg-white p-6 rounded-md shadow-sm gap-4">
        <h1 class="text-2xl p-4 text-center text-slate-600">Login</h1>

        <form @submit.prevent="signIn">
          <div class="flex flex-col gap-4">
            <div>
              <label for="email" class="block text-slate-500">Email</label>
              <input
                v-model="email"
                class="border py-2 px-3 rounded-lg w-full"
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
            <button
              type="submit"
              class="border py-2 px-3 rounded-lg w-full bg-green-800 hover:bg-green-700 text-white hover:shadow-md"
            >
              Login
            </button>
          </div>
        </form>
      </div>
    </section>
  </main>
</template>
