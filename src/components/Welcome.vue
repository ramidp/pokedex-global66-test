<script setup>

import { ref } from 'vue'


// Props and Emits
const prop = defineProps({
    theme: Object,
})

const emit = defineEmits({
    'unfade': Function,
    'fade' : Function,
})

// Variable to Display Welcome and UnDisplay
const welcomeDisplay = ref(true)

const delayedDisplay = () => {
    setTimeout(() => {
        welcomeDisplay.value = false
    },1000)
}

</script>

<template>
  
    <div 
    v-if="welcomeDisplay"
    class="welcome-container">
        
        <div class="welcome-banner">
            <img src="../assets/images/pikachu.png" alt="">
        </div>

        <h1>Welcome to PokéDex</h1>

        <p>The digital encyclopedia created by Professor Oak is an invaluable tool to Trainers in the Pokémon world.</p>

        <button
        @click="$emit('unfade'), delayedDisplay()"
        >Get started</button>

    </div>

</template>

<style lang='scss' scoped>

.welcome-container {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    flex-direction: column;
    gap: 4vh 0;
    height: calc(100% - 15vh);
    padding-top: 15vh;
    width: calc(100%);
    background-color: v-bind('prop.theme.ninth');


    @media (max-width: 1100px) {
        padding-top: 10vh;
        height: calc(100% - 10vh);
    }

    h1, p {
        margin: 0;
        padding: 0;
    }
    h1 {
        font-size: 26px;
    }
    p {
        font-size: 18px;
        width: 33%;
        text-align: center;

        @media (max-width: 1100px) {
        width: 90%
    }
    }
    
    .welcome-banner {
        display: flex;
        justify-content: center;
        align-items: center; 
        background-color: v-bind('prop.theme.first');
        border-radius: 50%;
        width: 260px;
        height: 260px;
        border: 10px solid black;

        @media (max-width: 1100px) {
            width: 200px;
            height: 200px;
    }
               
        img {
            height: 100%;
            position: relative;
            }
    }

    button {
        background-color: v-bind('prop.theme.first');
        border: none;
        color: white;
        border-radius: 25px;
        padding: 11px 20px;
        font-size: 16px;
        font-weight: 600;
        transition: 300ms ease all;

        &:hover {
            cursor: pointer;
            background-color: v-bind('prop.theme.second');
        }
    }

}

</style>
