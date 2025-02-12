<template>
  <div v-if="isOpen" class="fixed inset-0 flex items-center justify-center">
    <div class="bg-white p-5 rounded-lg shadow-lg max-w-md">
      <button
        @click="$emit('close')"
        class="float-right text-x1 bg-red-500 text-white px-2 py-1 rounded-md cursor-pointer font-bold"
      >
        X
      </button>
      <h2 class="text-2xl font-semibold">{{ character.name }}</h2>
      <img
        :src="character.image"
        :alt="character.name"
        class="rounded-lg shadow-lg my-3"
      />
      <p><strong>Species:</strong> {{ character.species }}</p>
      <p><strong>Gender:</strong> {{ character.gender }}</p>
      <p><strong>Location:</strong> {{ character.location.name }}</p>
      <p><strong>Status:</strong> {{ character.status }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isOpen: Boolean,
    character: Object,
  },
  emits: ["close"],
};
</script>

<style></style>
