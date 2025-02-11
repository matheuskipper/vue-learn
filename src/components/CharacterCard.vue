<template>
  <div class="bg-[#FFFFFF] p-4 rounded-lg shadow-lg">
    <img :src="image" :alt="name" class="rounded-lg shadow-lg" />
    <h2 class="text-xl font-semibold">{{ name }}</h2>
    <p>Status: {{ status }}</p>
  </div>
</template>

<script>
export default {
  props: {
    image: String,
    name: String,
    status: String,
  },
};
</script>

<style></style>
