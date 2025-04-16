<template>
    <div 
      style="background-color: #FFFFFF"
      class="relative rounded-lg p-4 text-center shadow-md hover:shadow-lg transition-shadow"
    >
      <img 
        :src="pokemon.image" 
        alt="Pokemon image" 
        class="w-50 h-50 mx-auto mb-2" 
        loading="lazy"
      />
      <h2 class="text-lg font-bold capitalize">{{ pokemon.name }}</h2>
      <h3 class="text-sm text-gray-500">{{ pokemon.id }}</h3>
  
      <button 
       @click.stop="emit('toggle-favorite', pokemon.name)" 
        class="absolute top-2 right-2">
        <HeartIcon 
          class="w-6 h-6"
          :class="isFavorite ? 'text-red-500' : 'text-gray-500'" 
        />
      </button>
    </div>
  </template>
  
<script setup>
import HeartIcon from '../assets/heart-solid.svg'

defineProps({
  pokemon: Object,
  isFavorite: Boolean
});

const emit = defineEmits(['toggle-favorite']);
</script>

<style>
svg {
  fill: currentColor;
}
</style>
  