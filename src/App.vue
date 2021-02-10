<template>
  <div id="app">   
    <div class="column is-half is-offset-one-quarter">
        <img src="./assets/pokemon-logo.jpg"><br>
        <input class="input is-info" type="text" placeholder="Buscar Pokemon pelo nome" v-model="busca">
        <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Pesquisar</button>
        <div v-for="(poke, index) in filtroPokemons" :key="poke.url">
            <Pokemon :name="poke.name" :url="poke.url" :num="index + 1"/>
        </div>
    </div>    
  </div>
</template>

<script>
import axios from 'axios'
import Pokemon from './components/Pokemon'
export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filtroPokemons: [],
      busca: ''
    }
  },
  created: function() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      this.pokemons = res.data.results
      this.filtroPokemons = res.data.results       
    })
  }, 
  components: {
    Pokemon
  },
  methods: {
    buscar: function(){
      this.filtroPokemons = this.pokemons
      if(this.busca == '' || this.busca == ' '){
        this.filtroPokemons = this.pokemons
      }else{
        this.filtroPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  } 
  // computed: {
  //   resultadoBusca: function() {
  //     if(this.busca == '' || this.busca == ' '){
  //       return this.pokemons
  //     }else{
  //       return this.pokemons.filter(pokemon => pokemon.name == this.busca)
  //     }
  //   }
  // } 
}
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
