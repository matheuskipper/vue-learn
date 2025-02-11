<template>
  <div class="w-[100%] flex flex-col justify center items-center bg-[#d5f5a2]">
    <h1
      id="titulo"
      class="text-5xl font-bold p-4 text-[#7FFFD4] text-shadow font-mono"
    >
      Personagens de Rick and Morty
    </h1>

    <input
      v-model="searchTerm"
      type="text"
      placeholder="Buscar personagem"
      class="border-2 border-[#7FFFD4] p-2 m-2 rounded-md shadow-lg w-[20rem] mb-5 mt-7"
    />

    <!--  Passando as props para o componente -->
    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 p-5">
      <CharacterCard
        v-for="character in paginatedCharacters"
        :key="character.id"
        :image="character.image"
        :name="character.name"
        :status="character.status"
      />
    </div>

    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      @prev="prevPage"
      @next="nextPage"
    />
  </div>
</template>

<script>
import axios from "axios";
import CharacterCard from "./components/CharacterCard.vue";
import Pagination from "./components/Pagination.vue";
export default {
  components: {
    CharacterCard,
    Pagination,
  },
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
