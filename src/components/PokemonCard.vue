<script setup>

import { ref, onMounted } from 'vue' 


//Props and Emits
const prop = defineProps({
    pokemon: Object,
    theme: Object,
})

const emit = defineEmits({
    'pokemonDisplayClose': Function,
    'refresh' : Function,
})


// Refs to store data
const pokemonData = ref([])
const typesMap = ref('')
const photo = ref('')
const pokeFavs = ref([])

// Api for every Pokemon
const apiUrl2 = `${prop.pokemon.url}`

// Function to Capitalize first Letter (Cause info is not capitalized)
function capitalizeFirstLetter(string) {
  return string.charAt(0).toUpperCase() + string.slice(1);
}

// onMounted to fetch and get data.
onMounted( () => {
    fetch(apiUrl2)
    .then(response => response.json())
    .then(data => {
       pokemonData.value = data;
        
       typesMap.value = pokemonData.value.types.map(item => capitalizeFirstLetter(item.type.name)).join(', ');
       photo.value = pokemonData.value.sprites.other['official-artwork'].front_default
    });

    const db = JSON.parse(localStorage.getItem('pokeFavsArray')) 
    if ( db != null) {
        pokeFavs.value = db
    } else {
        pokeFavs.value = []
    }
});


// Function to add the variable favs=true to the pokemon you want to add to your list

const addingToFavorites = () => {

    for (const item of pokeFavs.value) {
        if (item.name === prop.pokemon.name) {
            item.favs = true;
            localStorage.setItem('pokeFavsArray', JSON.stringify(pokeFavs.value))
        }
    }
    emit('pokemonDisplayClose')
    emit('refresh')
}

// Function to copyToClipboard

const copyToClipBoard = () => {

    const texto = 'Name: ' + capitalizeFirstLetter(`${prop.pokemon.name}`) 
    + ', Weight: ' + `${pokemonData.value.weight}` + ', Height: ' + `${pokemonData.value.height}`  + ', Types: ' + capitalizeFirstLetter(`${typesMap.value}`) 
    
    navigator.clipboard.writeText(texto)
      .then(() => {
        console.log('Info has been copied.')
      })
      .catch((error) => {       
        console.error("Error to copy info:", error); 
      });
}


</script>

<template>

<div
class="pokemon-info-bg">                            

<div 
class="pokemon-info-card">

    <img 
    @click="$emit('pokemonDisplayClose')"
    class="close"
    src="../assets/images/x-mark.png" alt="">
    
            <div class="pokemon-bg">
                <img :src="photo" alt="">
            </div>
            
            <ul>
                <li><span>Name: </span> {{ pokemon.name }}</li>
                <li><span>Weight: </span> {{ pokemonData.weight }}</li>
                <li><span>Height: </span> {{ pokemonData.height }}</li>
                <li><span>Types: </span> {{ typesMap }}</li>

            </ul>

            <div class="pokemon-card-btns">
                
                <h1
                @click="copyToClipBoard"
                >Share to my friends</h1>

                <img 
                @click="addingToFavorites"
                class="star"
                v-if="pokemon.favs != true"
                src="../assets/images/star-gray.png" alt="">

                <img 
                class="star"
                @click="addingToFavorites"
                v-if="pokemon.favs === true"
                src="../assets/images/star-color.png" alt="">   

            </div>

    </div>

</div>

</template>


<style lang='scss' scoped>

.pokemon-info-bg {
    width: 100%;
    height: 100%;
    background-color: rgba(128, 128, 128, 0.849);
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
    display: flex;
    justify-content: center;
    align-items: center;
    
    .close {
        position: absolute;
        padding-top: 20px;
        padding-right: 20px;
        transition: 300ms ease all;

        &:hover {
            cursor: pointer;
            filter: contrast(50%)
        }
    }
    .pokemon-info-card {
        background-color: white;
        width: 60%;
        height: 70%;
        display: flex;
        justify-content: space-between;
        flex-direction: column;
        align-items: flex-end;
        border-radius: 7.5px;
        z-index: 1000;

        @media (max-width: 1100px) {
            width: 75%;
            height: 80%;
        }

        ul {
            list-style: none;
            display: flex;
            justify-content: fl;
            align-items: center;
            gap: 2vh 0;
            flex-direction: column;
            width: 100%;
            height: auto;
            padding: 2vh 0;
            margin: 0;
            li {
                text-transform: capitalize;
                padding-bottom: 5px;
                width: 90%;
                font-size: calc(20px + 0.25vw);
                border-bottom: 1px solid gray;
                color: v-bind('prop.theme.fifth');

                span {
                    font-weight: bold;
                }
            }
        }
        
        .pokemon-bg {
            border-top-left-radius: 5px;
            border-top-right-radius: 5px;
            width: 100%;
            height: 50%;
            background-image: url('../assets/images/pokemon-bg.png');
            background-size: cover;
            background-repeat: no-repeat;     
            display: flex;
            justify-content: center;
            align-items: center;

            img {
                height: 70%;
                object-fit: contain;
            }
        }
        
        .pokemon-card-btns {
            display: flex;
            width: calc(100% - 4vw);
            justify-content: space-between;
            flex-direction: row;
            align-items: center;
            padding: 2vh 2vw;

                h1 {
                display: flex;
                justify-content: center;
                align-items: center;
                gap: 0 1vw;
                margin: 0;
                text-align: center;
                font-size: 20px;
                width: 200px;
                border-radius: 60px;
                color: white;               
                padding: 1vh 1vw;
                background-color: v-bind('prop.theme.first');
                transition: 300ms ease all;
                &:hover {
                        cursor: pointer;
                        filter: contrast(50%);
                    }
                }

                .star {
                        width: 25px;
                        background-color: v-bind('prop.theme.eighth');
                        padding: 7px;
                        border-radius: 50%;
                        transition: 300ms ease all;
                        &:hover {
                            cursor: pointer;
                            filter: contrast(80%);
                        }
                    }
                }
            }
        }

</style>