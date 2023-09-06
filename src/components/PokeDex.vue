<script setup>
import PokemonCard from './PokemonCard.vue'
import PokeFavs from './PokeFavs.vue'

import { onMounted, ref, computed, onUpdated} from 'vue'
 
// Props dn Emits

const prop = defineProps({
    theme: Object,
    componentKey: Number
})

const emit = defineEmits({
    'refresh' : Function,
})

// Ref Variables

const pokemons = ref([])
const pokemonName = ref('')
const apiUrl = 'https://pokeapi.co/api/v2/pokemon';
const pokeFavs2 = ref([])
const btnActive = ref(false)
const all = ref(true)

//onMounted to fetch pokemon data and store it, first in pokemons and then in localStorage

onMounted( () => {
    const result = fetch(apiUrl);
    result
    .then(response => response.json())
    .then(data => {
       pokemons.value = data.results;
       const db = JSON.parse(localStorage.getItem('pokeFavsArray')) 
       if (db === null) {
           localStorage.setItem('pokeFavsArray' , JSON.stringify(pokemons.value))
           pokeFavs2.value = pokemons.value
       } else {
           pokeFavs2.value = db
       }
  
    });
});

// Variable to select Pokemon for multiple uses

const selectingPokemon = (index) => {
    pokemonName.value = filteredPokemons.value[index].name;
    pokemonDisplay.value = true
}

// Search and Filtered Array with the Pokemon you want to look.

const searchedPokemon = ref('')

const filteredPokemons = computed(() => {    
    return pokeFavs2.value.filter(pokemon =>
    pokemon.name.toLowerCase().includes(searchedPokemon.value.toLowerCase())
    )
  
});

// To display Pokemon Cards
const pokemonDisplay = ref(false)


// Events to pass to children
const event = {
    'pokemonDisplayClose' : () => { pokemonDisplay.value = false},
    'refresh' : () => { emit('refresh') }
}

// Error in case you didn't get any pokemon by searching
const errorDisplay = ref(false)


</script>

<template>

        <div
        class="pokedex-container">        
            <div class="search-bar">

                <img src="../assets/images/search.png" alt="">

                <input
                    placeholder='Search'
                    type="text"
                    v-model="searchedPokemon"
                    />

            </div>
            <Transition>
            <div
            v-show="filteredPokemons.length > 0"
            class="pokemon-list"
            >            
                <div
                v-show="all"                
                class="pokemon-div"
                v-for="(pokemon, index) in filteredPokemons"  
                :key="pokemon.name"              
                >   
                    <div 
                    @click="selectingPokemon(index)"
                    class="pokemon-item">
                        <h1
                            >{{ pokemon.name }}</h1>
    
                            <img 
                            v-if="pokemon.favs !== true"
                            class="star"
                                src="../assets/images/star-gray.png" alt="">    

                            <img 
                            v-if="pokemon.favs === true"
                            class="star"
                                src="../assets/images/star-color.png" alt="">                                

                    </div>

                    <PokemonCard
                    v-show="pokemonDisplay && pokemonName == pokemon.name"
                    :pokemon="pokemon"  
                    :theme="theme"     
                    v-on="event"                
                    />
                        
                    
                </div>
                
                <PokeFavs
                v-if="!all"      
                :theme="theme"
                :pokemons="pokemons"    
                />                   
                

            </div>
        </Transition>

        <Transition
        >
            <div 
            v-if="filteredPokemons.length === 0"
            class="error-msg">
                <h1>Uh-oh!</h1>
                <p>You look lost on your journey!</p>
                <a href="/">Go back home</a>
            </div>
        </Transition>

            <div class="bottom-menu">

                <h1
                @click="btnActive = false, all = true"
                :class="btnActive ? '' : 'btnActive'"
                >
                <img src="../assets/images/all.png" alt="">
                All</h1>
                
                <h1
                @click="btnActive = true, all = false"
                :class="btnActive ? 'btnActive' : ''"
                >
                <img src="../assets/images/star.png" alt="">
                Favorites</h1>
            </div>
            
        </div>

</template>

<style lang="scss">

    .pokedex-container {
        position: fixed;
        width: 100%;
        height: 100vh;
        top: 0;
        left: 0;
        z-index: 1;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
        padding-top: 5vh;
        background: v-bind('prop.theme.ninth');

        .v-enter-active {
        transition: opacity 1s ease;
        }

        .v-enter-from,
        .v-leave-to {
        opacity: 0;
        }
                
            .search-bar {
            display: flex;
            flex-direction: row;
            align-items: center;
            justify-content: flex-start;
            border-radius: 5px;
            background-color: white;
            padding: 5px;
            width: 50%;
            height: 50px;

            @media (max-width: 1100px) {
                width: 90%;
            }
            
            img {
                width: 20px;
                object-fit: contain;
            }
            
    
            input {
                background-color: transparent;
                outline: none;
                border: none;
                height: 3vh;
                padding-left: 20px;
                font-size: 18px;
                font-weight: 600;
                width: 95%;
                
                &::placeholder {
                    color: rgba(128, 128, 128, 0.541);
                    font-family: Lato;
                }
            }
        }

        .pokemon-list {
            width: 50%;
            height: calc(82vh - 4vh);
            display: flex;
            justify-content: flex-start;
            align-items: center;
            flex-direction: column;
            overflow-y: auto;
            gap: 1vh 0;
            padding: 5px 0;   
            margin: 2vh 0;

            @media (max-width: 1100px) {
                width: 95%;
                height: calc(78vh - 4vh)
            }
            .pokemon-item {
                display: flex;
                justify-content: flex-start;
                align-items: center;
                width: calc(100% - 5vw);
                background-color: white;
                box-shadow: 0px 0px 5px 5px rgba(200,200,200,0.1);
                padding: 0 2.5vw;
                transition: 300ms ease all;

                
                &:hover {
                    cursor: pointer;
                    background-color: rgb(233, 233, 233);
                    filter: contrast(90%)
                }
                .star {
                    width: 25px;
                    background-color: v-bind('prop.theme.eighth');
                    padding: 7px;
                    border-radius: 50%;
                    transition: 300ms ease all;
                    }

                h1 {
                    margin: 0;
                    width: 90%;
                    padding: 2vh 0;
                    padding-left: 10px;
                    font-size: 20px;
                    line-height: 26px;
                    text-transform: capitalize;
                    color: v-bind('prop.theme.fourth');
                 
               
                }
            }

            .pokemon-div {   
                width: 100%;       

            }

        }
        .error-msg {
            display: flex;
            justify-content: center;
            align-items: center;
            width: 100%;
            flex-direction: column;
            z-index: -1;
            position: absolute;
            padding-top: 7vh;
            animation: entrance;
            animation-duration: 1s;
            animation-delay: 0.2s;
            filter: opacity(0);
            animation-fill-mode: forwards;

            @keyframes entrance {
                0% { filter: opacity(0)}
                100% { filter: opacity(1)}
            }

            a {
                text-decoration: none;
                color: white;
                border-radius: 25px;
                padding: 1.5vh 3vw;
                background-color: v-bind('prop.theme.first');
                transition: 300ms ease all;
                &:hover {
                    background-color: v-bind('prop.theme.second');
                }
            }
        }

        .bottom-menu {
            position: fixed;
            bottom: 0;
            left: 0;
            z-index: 10;
            width: 100%;
            box-shadow: 0px 0px 10px 5px rgba(200,200,200,0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 0 2vw;
            padding: 1vh 0;
            background-color: white;

            @media (max-width: 1100px) {
                    gap: 0 6vw;
                 }


            h1 {
                display: flex;
                justify-content: center;
                align-items: center;
                gap: 0 1vw;
                margin: 0;
                text-align: center;
                font-size: 20px;
                width: 150px;
                border-radius: 60px;
                color: white;               
                padding: 1vh 0.5vw;
                background-color: v-bind('prop.theme.sixth');
                transition: 300ms ease all;

                 @media (max-width: 1100px) {
                    font-size: 18px;
                    width: 130px;
                 }

                &:hover {
                        cursor: pointer;
                        filter: contrast(50%);
                    }
                    img {
                        width: 20px;
                    }
            }
            .btnActive {
                background-color: v-bind('prop.theme.first');
            }
            
        }
    }

</style>