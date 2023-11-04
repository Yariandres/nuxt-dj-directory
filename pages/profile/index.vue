<script setup lang="ts">
const supabase = useSupabaseClient();
const router = useRouter();
definePageMeta({
  middleware: ['auth'],
});

const loading = ref(true);
const username = ref('');
const website = ref('');
const bio = ref('');
const avatar_path = ref('');

loading.value = true;
const user = useSupabaseUser();

const { data } = await supabase
  .from('profiles')
  .select(`username, website, avatar_url, bio`)
  .eq('id', user.value.id)
  .single();

if (data) {
  username.value = data.username;
  website.value = data.website;
  bio.value = data.bio;
  avatar_path.value = data.avatar_url;
}

loading.value = false;

async function updateProfile() {
  try {
    loading.value = true;
    const user = useSupabaseUser();

    const updates = {
      id: user.value?.id,
      username: username.value,
      website: website.value,
      avatar_url: avatar_path.value,
      bio: bio.value,
      updated_at: new Date(),
    };

    const { error } = await supabase.from('profiles').upsert(updates, {
      returning: 'minimal', // Don't return the value after inserting
    });
    if (error) throw error;
  } catch (error: any) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}

async function signOut() {
  try {
    loading.value = true;
    const { error } = await supabase.auth.signOut();
    if (error) throw error;
    user.value = null;
  } catch (error: any) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}
</script>
<template>
  <main class="h-screen bg-slate-900">
    <section class="mx-auto px-9 pt-9">
      <div class="rounded-lg bg-slate-900">
        <form class="form-widget" @submit.prevent="updateProfile">
          <div class="p-9 flex flex-col gap-4">
            <profile-avatar
              v-model:path="avatar_path"
              @upload="updateProfile"
            />
            <div>
              <label for="email" class="block text-slate-100">Email</label>
              <input
                class="border py-2 px-3 rounded-lg w-full"
                id="email"
                type="text"
                :value="user?.email"
                disabled
              />
            </div>
            <div>
              <label for="username" class="block text-slate-100"
                >Username</label
              >
              <input
                class="border py-2 px-3 rounded-lg w-full bg-transparent text-slate-100"
                id="username"
                type="text"
                v-model="username"
              />
            </div>
            <div>
              <label for="website" class="block text-slate-100">Website</label>
              <input
                class="border py-2 px-3 rounded-lg w-full bg-transparent text-slate-100"
                id="website"
                type="url"
                v-model="website"
              />
            </div>

            <div>
              <label for="website" class="block text-slate-100">Bio</label>
              <input
                class="border py-2 px-3 rounded-lg w-full bg-transparent text-slate-100"
                id="website"
                type="text"
                v-model="bio"
              />
            </div>

            <div class="text-right">
              <input
                type="submit"
                class="border py-2 px-3 mr-3 rounded-lg bg-green-900 hover:bg-green-700 text-white hover:shadow-md"
                :value="loading ? 'Loading ...' : 'Update'"
                :disabled="loading"
              />

              <button
                class="border py-2 px-3 rounded-lg bg-blue-900 hover:bg-blue-700 text-white hover:shadow-md"
                @click="signOut"
                :disabled="loading"
              >
                Sign Out
              </button>
            </div>
          </div>
        </form>
      </div>
    </section>
  </main>
</template>
