<script setup>
import { ref, onMounted } from 'vue'

const prop = defineProps({
    theme: Object,
    pokemons: Array,
})

const emit = defineEmits({
    'pokemonDisplayClose': Function,
})


//  pokeFavsArray to storage all the info and to use in the future to store the variable fav.
const pokeFavs = ref([])

// onMounted to store some data in LocalStorage
onMounted( () => {
    const db = JSON.parse(localStorage.getItem('pokeFavsArray')) 
    if ( db != null) {
        pokeFavs.value = db
    } else {
        pokeFavs.value = []
    }
});

</script>

<template>

<div
class="pokemon-div"
v-for="(pokemon) in pokeFavs.filter(item => item.favs === true)"
>    
    <div     
    :key="pokemon.name"  
    class="pokemon-item">

        <h1
        >{{ pokemon.name }}</h1>
        
        <img 
        class="star"
        src="../assets/images/star-color.png" alt="">
        
    </div>   
</div>


</template>

<style lang='scss'>


</style>