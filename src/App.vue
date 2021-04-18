<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row align-items-center">
        <div class="input-wrapper text-center col-12 col-md-6 col-lg-6">
          <h1 class="title">{{ getName }}</h1>
          <img class="images" width="200px" :src="getImageFront" alt="" />
          <img class="images" width="200px" :src="getImageBack" alt="" />
          <div class="row">
            <input
              class="form-control col-4 me-2"
              style="width: 300px"
              v-model="nombre_pokemon"
              type="text"
            />
            <button class="btn btn-danger col-3" @click="getData">
              Search
            </button>
          </div>
        </div>
        <div class="info-wrapper col-12 col-md-6 col-lg-6 mt-4">
          <ul class="row mb-5">
            <h2 class="mb-4">Abilities</h2>
            <li
              class="items col-3"
              v-for="(pokeAbility, i) in getAbilities"
              :key="i"
            >
              {{ pokeAbility.ability.name }}
            </li>
          </ul>
          <ul class="row moves-wrapper">
            <h2 class="mb-4">Moves</h2>
            <li
              class="items col-3 mt-2"
              v-for="(pokeMove, i) in getMoves"
              :key="i"
            >
              {{ pokeMove.move.name }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      nombre_pokemon: "pikachu",
      pokeData: "",
    };
  },
  methods: {
    async getData() {
      const url = "https://pokeapi.co/api/v2/pokemon/";
      try {
        const req = await axios(url + this.nombre_pokemon);
        if (!req) return;
        this.pokeData = req.data;
      } catch (error) {
        console.log(error);
      }
    },
  },
  created() {
    this.getData();
  },
  computed: {
    getName() {
      return this.pokeData.name;
    },
    getImageFront() {
      return (
        this.pokeData &&
        this.pokeData.sprites &&
        this.pokeData.sprites.front_default
      );
    },
    getImageBack() {
      return (
        this.pokeData &&
        this.pokeData.sprites &&
        this.pokeData.sprites.back_default
      );
    },
    getMoves() {
      return this.pokeData.moves;
    },
    getAbilities() {
      return this.pokeData.abilities;
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  color: #2c3e50;
  height: 100%;
}

h1,
h2 {
  font-weight: bold;
}

ul > li {
  list-style: none;
  font-size: 0.7rem;
}

.moves-wrapper {
  height: 290px;
  overflow-y: hidden;
}
</style>
