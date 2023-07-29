<script>

import PokemonCards from './components/PokemonCards.vue'

const api = 'https://pokeapi.co/api/v2/pokemon'
const pokemonIds = [1, 3, 5]

export default{
  components: {
    PokemonCards
  },

  data(){
    return {
      pokemons: [],
      evolutions: [],
      selectedId: null
    }
  },

  async created(){
    this.pokemons = await this.fetchData(pokemonIds)
  },

  methods: {
    async fetchDatum(){
      const response = await window.fetch(`${api}/1`)
      const json = await response.json();
      this.pokemons = {
        id: json.id,
        name: json.name,
        sprite: json.sprites.other['official-artwork'].front_default,
        types: json.types.map(type => type.type.name)
      }
    },

    async fetchEvolutions(pokemon){
      this.evolutions = await this.fetchData([pokemon.id + 1, pokemon.id + 2])
      this.selectedId = pokemon.id
    },

    async fetchData(ids){
      const responses = await Promise.all(
        ids.map(id => window.fetch(`${api}/${id}`))
      )

      const json = await Promise.all(
        responses.map(data => data.json())
      )

      return json.map(datum => ({
        id: datum.id,
        name: datum.name,
        sprite: datum.sprites.other['official-artwork'].front_default,
        types: datum.types.map(type => type.type.name)
      }))
    }
  },

  
}
</script>

<template>
  
  <pokemon-cards
    :pokemons="pokemons"
    @chosen="fetchEvolutions"
    :selectedId="selectedId"
  />

  <pokemon-cards
    :pokemons="evolutions"
  />

</template>

<style scoped>

</style>
