<template>
  <section class="h-screen flex justify-center pt-[100px]">
    <div class="flex flex-col gap-6">
      <h1
        class="text-6xl text-slate-200 uppercase font-bold text-center w-[590px] text-transparent bg-clip-text bg-gradient-to-r from-indigo-600 to-red-600"
      >
        Find top djs for your in your area
      </h1>

      <div class="flex flex-col gap-3">
        <label for="location" class="text-3xl text-slate-300 text-center"
          >Enter your location</label
        >
        <input
          class="border-2 py-2 px-3 rounded-lg w-full bg-transparent outline-none focus:border-violet-500"
          id="location"
          type="text"
          placeholder="Enter your location..."
        />
      </div>
    </div>
  </section>
</template>

<script setup lang="ts"></script>
