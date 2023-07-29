<script>
    import Card from './Card.vue'

    export default {

        props: {
            pokemons: {
                type: Array,
                default: []
            },
            selectedId: {
                type: Number
            }
        },

        components: {
            Card
        },

        methods: {
            click(pokemon){
                this.$emit("chosen", pokemon)
            }
        }
    }
</script>

<template>
    <div class="cards">
        <card
        v-for="pokemon in pokemons"
        :key="pokemon.id"
        :class="{opace: pokemon.id !== selectedId}"
        class="card"
        @click="click(pokemon)">

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

.opace {
  opacity: 0.5;
}

.card:hover{
  opacity: 1.0;
}
</style>