<template>
  <div class="w-[100%] flex flex-col justify center items-center bg-[#d5f5a2]">
    <h1 id="titulo" class="text-5xl font-bold p-4 text-[#7FFFD4] text-shadow">
      Personagens de Rick and Morty
    </h1>

    <input
      v-model="searchTerm"
      type="text"
      placeholder="Buscar personagem"
      class="border-2 border-[#7FFFD4] p-2 m-2 rounded-md shadow-lg w-[20rem] mb-5"
    />

    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 p-5">
      <div
        v-for="character in filteredCharacters"
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
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      characters: [],
      searchTerm: "",
    };
  },
  async created() {
    try {
      const response = await axios.get(
        "https://rickandmortyapi.com/api/character"
      );
      this.characters = response.data.results; // Pegando os personagens
      console.log(this.characters);
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
  },
};
</script>

<style scoped>
#titulo {
  text-shadow: 2px 2px 4px #000000;
}
</style>
