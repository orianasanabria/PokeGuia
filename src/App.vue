<template>
  <div id="app">
    <label for="input">Nombre: </label>
    <input v-model="nombre_pokemon" type="text" />
    <button @click="getData">Buscar</button>
    <h1 class="title">{{ getName }}</h1>
    <img :src="getImage" alt="" />
    <h2>Movimientos</h2>
    <ul>
      <li v-for="(pokeMove, i) in getMoves" :key="i">
        {{ pokeMove.move.name }}
      </li>
    </ul>
    <h2>Habilidades</h2>
    <ul>
      <li v-for="(pokeAbility, i) in getAbilities" :key="i">
        {{ pokeAbility.ability.name }}
      </li>
    </ul>
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
        console.log(req.data);
        this.pokeData = req.data;
        console.log(this.pokeData);
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
    getImage() {
      return (
        this.pokeData &&
        this.pokeData.sprites &&
        this.pokeData.sprites.front_default
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
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
