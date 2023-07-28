<script>
import Card from './components/Card.vue'

const api = 'https://pokeapi.co/api/v2/pokemon'
const ids = [1, 3, 5]

export default{
  components: {
    Card
  },

  data(){
    return {
      pokemons: []
    }
  },

  created(){
    this.fetchData()
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

    async fetchData(){
      const responses = await Promise.all(
        ids.map(id => window.fetch(`${api}/${id}`))
      )

      const json = await Promise.all(
        responses.map(data => data.json())
      )

      this.pokemons = json.map(datum => ({
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
  <div class="cards">
    <card
      v-for="pokemon in pokemons"
      :key="pokemon.id">

      <template v-slot:title>
        {{ pokemon.title }}
      </template>

      <template v-slot:content>
        <img :src="pokemon.sprite" />
      </template>

      <template v-slot:description>
        <div v-for="type in pokemon.types" :key="type">
          {{ type }}
        </div>
      </template>
    </card>
  </div>
</template>

<style scoped>

.cards{
  display: flex;
}

img{
  width: 100%;
}
</style>
