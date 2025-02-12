<template>
  <div
    class="w-full min-h-screen flex flex-col justify center items-center bg-[#d5f5a2]"
  >
    <h1
      id="titulo"
      class="text-[66px] font-bold p-4 text-[#7FFFD4] text-shadow font-mono"
    >
      Rick & Morty - Characters
    </h1>

    <input
      v-model="searchTerm"
      type="text"
      placeholder="Search character"
      class="border-2 border-[#4F4F4F] p-2 m-2 rounded-md shadow-lg w-[20rem] mb-5 mt-7"
    />

    <div v-if="loading" class="text-center text-2xl font-bold mt-10">
      <div
        class="animate-spin rounded-full h-10 w-10 border-t-4 border-b-4 border-black"
      ></div>
      Loading...
    </div>

    <!--  Passando as props para o componente -->
    <div v-else class="grid grid-cols-2 md:grid-cols-4 gap-4 p-5">
      <CharacterCard
        v-for="character in paginatedCharacters"
        :character="character"
        @click="openModal(character)"
        class="cursor-pointer"
      />
    </div>

    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      @prev="prevPage"
      @next="nextPage"
    />

    <CharacterModal
      v-if="selectedCharacter"
      :character="selectedCharacter"
      :isOpen="modalOpen"
      @close="closeModal"
    />
  </div>
</template>

<script>
import axios from "axios";
import CharacterCard from "./components/CharacterCard.vue";
import Pagination from "./components/Pagination.vue";
import CharacterModal from "./components/CharacterModal.vue";
export default {
  components: {
    CharacterCard,
    Pagination,
    CharacterModal,
  },
  data() {
    return {
      characters: [],
      searchTerm: "",
      currentPage: 1,
      itemsPerPage: 8,
      selectedCharacter: null,
      modalOpen: false,
      loading: true,
    };
  },
  async created() {
    try {
      const response = await axios.get(
        "https://rickandmortyapi.com/api/character"
      );
      this.characters = response.data.results; // Pegando os personagens
      this.totalPages = Math.ceil(this.characters.length / this.itemsPerPage);

      setTimeout(() => {
        this.loading = false;
      }, 1000);
    } catch (error) {
      console.error("Erro ao buscar personagens:", error);
    } finally {
      this.loading = false;
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
    openModal(character) {
      this.loading = true;
      setTimeout(() => {
        this.selectedCharacter = character;
        this.modalOpen = true;
        this.loading = false;
      }, 500);
    },
    closeModal() {
      this.selectedCharacter = null;
      this.modalOpen = false;
    },
    performSearch() {
      this.loading = true;
      setTimeout(() => {
        this.currentPage = 1;
        this.loading = false;
      }, 1000);
    },
  },
  watch: {
    searchTerm() {
      this.performSearch();
    },
  },
};
</script>

<style scoped>
#titulo {
  text-shadow: 2px 2px 4px #000000;
}
</style>
