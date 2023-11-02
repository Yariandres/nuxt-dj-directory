<script setup lang="ts">
const props = defineProps(['path']);
const { path } = toRefs(props);

const emit = defineEmits(['update:path', 'upload']);
const supabase = useSupabaseClient();

const uploading = ref(false);
const src = ref('');
const files = ref();

const downloadImage = async () => {
  try {
    const { data, error } = await supabase.storage
      .from('avatars')
      .download(path.value);
    if (error) throw error;
    src.value = URL.createObjectURL(data);
  } catch (error) {
    console.error('Error downloading image: ', error.message);
  }
};

const uploadAvatar = async (evt) => {
  files.value = evt.target.files;
  try {
    uploading.value = true;

    if (!files.value || files.value.length === 0) {
      throw new Error('You must select an image to upload.');
    }

    const file = files.value[0];
    const fileExt = file.name.split('.').pop();
    const fileName = `${Math.random()}.${fileExt}`;
    const filePath = `${fileName}`;

    const { error: uploadError } = await supabase.storage
      .from('avatars')
      .upload(filePath, file);

    if (uploadError) throw uploadError;

    emit('update:path', filePath);
    emit('upload');
  } catch (error) {
    alert(error.message);
  } finally {
    uploading.value = false;
  }
};

downloadImage();

watch(path, () => {
  if (path.value) {
    downloadImage();
  }
});
</script>
<template>
  <div class="flex items-center">
    <img
      v-if="src"
      :src="src"
      alt="Avatar"
      class="mb-6"
      style="width: 10em; height: 10em"
    />
    <div
      v-else
      class="bg-gray-100 rounded-lg"
      style="height: 10em, width: 10em"
    />

    <div style="width: 10em; position: relative">
      <label
        for="single"
        class="cursor-pointer border py-2 px-3 rounded-lg w-full bg-green-800 hover:bg-green-700 text-white hover:shadow-md"
      >
        {{ uploading ? 'Uploading ...' : 'Upload' }}
      </label>
      <input
        style="position: absolute; visibility: hidden"
        type="file"
        id="single"
        accept="image/*"
        @change="uploadAvatar"
        :disabled="uploading"
      />
    </div>
  </div>
</template>
