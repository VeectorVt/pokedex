<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img
        src="./assets/OSkIXgBSMGsQ7XYX6bsI_LOGOTIPO-CODER-FUNDOTRANSPARENTE-PRETA.png"
      />
      <hr />
      <h4 class="is-size-4">Pokedex</h4>
      <input
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="busca"
        class="input is-rounded"
      />
      <button @click="buscar" class="button is-fullwidth is-success" id="buscaBtn">
        Buscar
      </button>
      <div v-for="(poke, index) in filteredPokemons " :key="poke.url">
        <pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import pokemon from "./components/pokemon.vue";
export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [ ],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Pegou a lista de Pokemons");
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
      });
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if(this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.name.includes(this.busca.toLowerCase())
        );
      }
    },
  },
  components: {
    pokemon,
  },
  // computed:{
  //   resultadoBusca:function(){
  //      if(this.busca == '' || this.busca == ' '){
  //        return this.pokemons
  //      }else{
  //        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
  //      }
  //   }
  // }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#buscaBtn {
  margin-top: 2%;
}
</style>
