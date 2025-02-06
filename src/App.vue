<template>
  <div class="w-[100%] flex flex-col justify center items-center bg-[#d5f5a2]">
    <h1 id="titulo" class="text-5xl font-bold p-4 text-[#7FFFD4] text-shadow">
      Personagens de Rick and Morty
    </h1>

    <input
      v-model="searchTerm"
      type="text"
      placeholder="Buscar personagem"
      class="border-2 border-[#7FFFD4] p-2 m-2 rounded-md shadow-lg w-[20rem] mb-5 mt-7"
    />

    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 p-5">
      <div
        v-for="character in paginatedCharacters"
        :key="character.id"
        class="bg-[#FFFFFF] p-4 rounded-lg shadow-lg"
      >
        <img
          :src="character.image"
          :alt="character.name"
          class="rounded-lg shadow-lg"
        />
        <h2 class="text-xl font-semibold">{{ character.name }}</h2>
        <p>Status: {{ character.status }}</p>
      </div>
    </div>
    <div class="flex justify-center mt-5 gap-2 mb-5">
      <button
        @click="prevPage"
        :disabled="currentPage === 1"
        class="px-4 py-2 bg-gray-500 text-white rounded-md"
      >
        Anterior
      </button>
      <span class="text-lg font-semibold">
        Página {{ currentPage }} de {{ totalPages }}
      </span>
      <button
        @click="nextPage"
        :disabled="currentPage === totalPages"
        class="px-4 py-2 bg-gray-500 text-white rounded-md"
      >
        Próxima
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      characters: [],
      searchTerm: "",
      currentPage: 1,
      itemsPerPage: 8,
    };
  },
  async created() {
    try {
      const response = await axios.get(
        "https://rickandmortyapi.com/api/character"
      );
      this.characters = response.data.results; // Pegando os personagens
      this.totalPages = Math.ceil(this.characters.length / this.itemsPerPage);
    } catch (error) {
      console.error("Erro ao buscar personagens:", error);
    }
  },
  computed: {
    filteredCharacters() {
      return this.characters.filter((character) =>
        character.name.toLowerCase().includes(this.searchTerm.toLowerCase())
      );
    },
    paginatedCharacters() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.filteredCharacters.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.filteredCharacters.length / this.itemsPerPage);
    },
  },
  methods: {
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
  },
  watch: {
    searchTerm() {
      this.currentPage = 1;
    },
  },
};
</script>

<style scoped>
#titulo {
  text-shadow: 2px 2px 4px #000000;
}
</style>
