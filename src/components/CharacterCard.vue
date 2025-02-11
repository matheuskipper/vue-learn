<template>
  <div class="bg-[#FFFFFF] p-4 rounded-lg shadow-lg">
    <img
      :src="character.image"
      :alt="character.name"
      class="rounded-lg shadow-lg mb-3"
    />
    <h2 class="text-xl font-semibold">{{ character.name }}</h2>
    <!-- <p>Status: {{ character.status }}</p> -->
  </div>
</template>

<script>
export default {
  props: {
    character: Object,
  },
};
</script>

<style></style>
