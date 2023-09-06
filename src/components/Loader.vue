<script setup>
import {onMounted, ref} from 'vue'


// Props and Emits
const prop = defineProps({
    theme: Object,
    fading: Boolean,
})

const emit = defineEmits({
    'unfade': Function,
    'fade' : Function,
    'pokedex' : Function,
})

const fading2 = ref(false)


// onMounted to setTimeouts for Animations
onMounted(() => {

    setTimeout(() => {
        fading2.value = true
    }, 3100);
    
    setTimeout(() => {
            emit('fade') ;
            emit('pokedex');
    }, 4000)
})

</script>

<template>  
    <div 
    :class="fading2? 'fading2 loader-container' : 'loader-container'"
    >
       <img src="../assets/images/pokeball.png" alt="">
    </div>

</template>

<style lang="scss" scoped>

.loader-container {
    width: 100%;
    height: 100vh;
    position: fixed;
    z-index: 1;
    top: 0;
    left: 0;
    background-color: v-bind('prop.theme.ninth');
    display: flex;
    justify-content: center;
    align-items: center;
    animation-name: fade;
    animation-duration: 1s;

    @keyframes fade {
        0% { filter: opacity(0)}
        100% { filter: opactiy(1)}
    }

    img {
        width: 106px;
        aspect-ratio: 1/1;
        animation-name: zoom-rotation;
        animation-duration: 2s;
        animation-iteration-count: infinite;

        @keyframes zoom-rotation {
            0% {}
            100% { transform: rotate(360deg)}
        }
    }
}

.fading2 {
    animation-name: fade2;
    animation-duration: 1s;

    @keyframes fade2 {
        0% { filter: opacity(1)}
        100% {filter: opacity(0)}
    }
}


</style>
