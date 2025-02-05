<template>
  <div class="container">
    <h1 id="titulo" class="text-3xl font-bold p-4">
      Personagens de Rick and Morty
    </h1>
    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 p-5">
      <div v-for="character in characters" :key="character.id" class="card">
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
};
</script>

<style scoped>
.container {
  text-align: center;
  background: #d5f5a2;
  width: 100vw;
  margin: 0;
  padding: 0;
}
.card {
  background: white;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
#titulo {
  color: aquamarine;
  text-shadow: 2px 2px 4px #000000;
}
</style>
