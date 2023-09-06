F<script setup>
import Loader from './components/Loader.vue'
import PokeDex from './components/PokeDex.vue';
import Welcome from './components/Welcome.vue';
import { ref } from 'vue'

const theme =   {  
    first: '#F22539',
    second: '#C00E20',
    third: '#ECA539',
    fourth: '#353535',
    fifth: '#5E5E5E',
    sixth: '#BFBFBF',
    seventh: '#E8E8E8',
    eighth: '#F5F5F5',
    ninth: '#F9F9F9',
  }

const fading = ref(false)
const pokedex = ref(true)
const componentKey = ref(0)

const event = {
  'unfade' : () => { fading.value = true},
  'fade' : () => { fading.value = false},
  'pokedex' : () => { pokedex.value = true},
  'refresh' : () => { 
    setTimeout(() => {
      componentKey.value += 1       
    }, 200)
  }
}


</script>

<template>

    <Welcome
    :theme="theme"
    v-on="event"
    />  

    <Loader
    v-if="fading"
    :theme="theme"
    v-on="event"
    :fading="fading"
    />
  
    <PokeDex
    v-if="pokedex"
    :theme="theme"
    :key="componentKey"
    v-on="event"
    />
  


</template>

<style scoped>


</style>
