<template>
  <div class="flex justify-center mt-5 gap-2 mb-18 mt-10">
    <button
      @click="$emit('prev')"
      :disabled="currentPage === 1"
      class="px-4 py-2 bg-[#00ad6b] hover:bg-[#006d43] text-white font-bold rounded-md cursor-pointer"
    >
      Anterior
    </button>
    <span class="text-lg font-semibold">
      Página {{ currentPage }} de {{ totalPages }}
    </span>
    <button
      @click="$emit('next')"
      :disabled="currentPage === totalPages"
      class="px-4 py-2 bg-[#00ad6b] hover:bg-[#006d43] text-white font-bold rounded-md cursor-pointer"
    >
      Próxima
    </button>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: Number,
    totalPages: Number,
  },
  emits: ["prev", "next"],
};
</script>

<style></style>
